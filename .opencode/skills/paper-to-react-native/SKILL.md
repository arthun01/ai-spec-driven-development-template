---
name: paper-to-react-native
description: >-
  Converte designs do Paper (paper.design) em código React Native idiomático: Componentes Funcionais,
  TypeScript, e NativeWind. Lê o design via Paper MCP (get_jsx,
  get_computed_styles, get_screenshot) e produz código React Native seguindo os padrões
  do projeto (NativeWind, Componentização, Zustand).
  REQUER: Paper Desktop app rodando + MCP Paper configurado no editor.
  Use quando o usuário pedir "implemente o design do Paper", "converta esse
  artboard em React Native", "gere o código a partir do Paper".
  Pode ser invocada diretamente ou pela skill executa-task quando a task tem
  referências de artboards Paper em seu arquivo de task ou em design.md.
  QUANDO NÃO: Use sem Paper disponível. Para criar designs no Paper (ver design-in-paper).
license: MIT
compatibility: Paper Desktop app + Paper MCP, React Native, Expo, NativeWind, TypeScript
---

Você é um engenheiro Mobile especialista em converter designs do Paper em código React Native idiomático.
Lê designs via Paper MCP e produz TSX + NativeWind seguindo os padrões do projeto.

## ⚠️ Pré-requisito Obrigatório

Antes de qualquer ação, verifique se o Paper MCP está disponível:

1. Chame `get_basic_info` para verificar a conexão com o Paper Desktop
2. Se falhar: informe o usuário que o Paper Desktop precisa estar aberto e o MCP configurado
3. Consulte: https://paper.design/docs/mcp para instruções de setup

Se o MCP não estiver disponível, **interrompa** — não tente gerar código sem o design.

> **📖 Referência de Tools:** Para documentação completa de todas as tools do Paper MCP
> (parâmetros, retornos, exemplos, workflows), consulte a skill **`mcp-paper`**.

---

## Como Esta Skill é Acionada

### Diretamente pelo usuário
```
"Converta o artboard 'orders / Lista de Pedidos' em React Native"
"Implemente o design do Paper para a feature de pedidos"
```

### Pela executa-task (automático)
A `executa-task` ativa esta skill quando detecta no arquivo de task:
```markdown
## Design de Referência (Paper)
- Artboard: orders / Lista de Pedidos / desktop
- Artboard ID: [id]
```
Ou quando existe `ai-sdd/prd-[feature-slug]/design.md` com pelo menos uma linha da tabela
`## Telas` tendo `Origem = Paper`.

---

## Procedimentos

### Passo 1: Identificar Artboard-Alvo

1. Se chamado via `executa-task`: leia `design.md` e identifique as linhas da tabela `## Telas`
   com `Origem = Paper` que sejam relevantes para a task atual. O ID do artboard está na coluna
   **Referência**.
2. Se chamado diretamente: peça ao usuário o nome ou ID do artboard
3. Verifique no Paper com `get_selection` (se o usuário tiver algo selecionado) ou use o nome para localizar via `get_basic_info`

### Passo 2: Inspecionar o Design

Execute estas chamadas em sequência:

```
1. get_basic_info          → confirmar arquivo aberto no Paper
2. get_selection           → ver se há seleção ativa (usar como ponto de partida)
3. get_tree_summary(id)    → entender hierarquia de layers do artboard
4. get_screenshot(id)      → capturar visual para referência
5. get_jsx(id)             → obter JSX + Tailwind do artboard
6. get_computed_styles(ids[]) → CSS computado dos nós principais
```

**Analise o output antes de gerar código:**
- Identificar subcomponentes lógicos que devem ser isolados em arquivos distintos
- Mapear textos estáticos
- Identificar dados dinâmicos (loops, propriedades)
- Detectar interações (estados locais, cliques)

### Passo 3: Planejar os Arquivos React Native

Antes de gerar o código, apresente o plano:

```
Arquivos que serão gerados/modificados:

Componentes:
  src/pages/OrdersPage.tsx                (Tela principal)
  src/components/Orders/OrderRow.tsx      (Subcomponente — linha da lista)
  src/components/Shared/EmptyState.tsx    (Subcomponente genérico — se não existir)

Tipagem (se necessário):
  src/types/orders.ts                     (Interface de dados do pedido)

Confirma que posso prosseguir?
```

Aguarde confirmação antes de gerar os arquivos.

### Passo 4: Converter JSX/Paper → TSX React Native

Regras de conversão obrigatórias:

#### Estrutura TSX
```jsx
// JSX do Paper (pode vir como mobile tags)
<div className="flex flex-col gap-6 p-8 bg-surface">
  <h1 className="text-2xl font-semibold">Lista de Pedidos</h1>
</div>
```
↓
```tsx
// TSX Final (React Native com NativeWind)
import { View, Text } from 'react-native';

export const OrdersPage = () => {
  return (
    <View className="flex-col gap-6 p-8 bg-surface">
      <Text className="text-2xl font-semibold text-text-primary">
        Lista de Pedidos
      </Text>
    </View>
  );
};
```

#### Regras de Conversão

| JSX/Paper | TSX React Native |
|-----------|-----------|
| Componente Grande | Quebrar em múltiplos subcomponentes funcionais com props (`interface Props`) |
| Cores hardcoded (`#3b82f6`) | NativeWind CSS classes (ex: `bg-blue-600` ou variáveis do tema) |
| SVG icons | Usar bibliotecas compatíveis com RN (ex: `lucide-react-native` ou `react-native-svg`) |
| `list.map(item => ...)` | No RN prefira `FlatList` para listas grandes ou map em `View` menores, usando `key={item.id}` |
| Interatividade (Click/Hover) | Adicionar `onPress` (usando `TouchableOpacity` ou `Pressable`) e `useState` para estado local. |

#### Dados Dinâmicos e Tipagem
Sempre defina uma `interface` para os dados dinâmicos do componente:
```tsx
interface OrderRowProps {
  order: {
    id: string;
    status: 'pending' | 'confirmed';
    total: number;
  };
  onActionPress: (id: string) => void;
}

export const OrderRow = ({ order, onActionPress }: OrderRowProps) => {
  return (
    <View className="border p-4 flex-row justify-between">
       {/* render order data */}
    </View>
  )
}
```

### Passo 5: Adaptar Cores para Design System (Tailwind)

O JSX do Paper pode ter cores não padronizadas. Tente utilizar cores padrões do Tailwind:
Se o template usar variáveis nativas (ex: `bg-(--color-surface)`), mantenha o formato de variáveis CSS.

### Passo 6: Validação Final

Após gerar todos os arquivos:

1. **Comparação visual** — chame `get_screenshot` novamente e compare mentalmente com o TSX gerado
2. **Checklist de qualidade:**
   - [ ] Props tipadas estritamente (sem `any`)
   - [ ] Subcomponentes lógicos separados
   - [ ] Uso de tags React Native (`View`, `Text`, `Pressable`, etc) em vez de HTML (`div`, `span`)
   - [ ] Uso de NativeWind de forma idiomática
   - [ ] Estados e side-effects mínimos e precisos.

---

## Integração com executa-task

Quando acionada por `executa-task`, esta skill:

1. É carregada no **Passo 2** da executa-task
2. Executa durante o **Passo 5** (Implementação) para gerar TSX
3. Entrega: Componentes Funcionais React Native em `src/components` ou `src/pages`
4. A `executa-task` continua integrando os estados e APIs

---

## Fallback: Sem Paper Disponível

Se o Paper MCP não estiver disponível durante `executa-task`, a skill informa:

> ⚠️ Paper MCP não detectado. Os componentes serão implementados com base na Tech Spec
> e no Tailwind, sem referência visual direta.
> Para usar o Paper, certifique-se que o Paper Desktop está aberto e o MCP configurado.

A `executa-task` prossegue normalmente sem o Paper.
