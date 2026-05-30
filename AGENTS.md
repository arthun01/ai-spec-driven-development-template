# Diretrizes de Desenvolvimento

> [Descrição curta do projeto]
> **React 18+ · TypeScript · Vite · Tailwind CSS · Zustand · React Query**

## Stack

- **Linguagem:** TypeScript
- **Frontend:** React 18+, Vite, Tailwind CSS
- **Estado Global:** Zustand
- **Data Fetching:** React Query (TanStack Query)
- **Testes:** Vitest + React Testing Library
- **Linting/Formatação:** ESLint + Prettier
- **Roteamento:** React Router (ou o roteador padrão do framework se usar Next.js futuramente)

## Arquitetura

```
src/
  components/  # Componentes reutilizáveis de UI e layout genérico.
  hooks/       # Custom hooks lógicos compartilhados.
  pages/       # Componentes de página/roteamento. Agrupam a lógica principal da view.
  services/    # Integração de API, clientes HTTP (axios/fetch).
  store/       # Definição de stores globais com Zustand.
  types/       # Definições globais de interfaces e tipos TypeScript.
  utils/       # Funções auxiliares e helpers puros.
```

## Regras Absolutas

| Regra | Detalhe |
|-------|---------|
| ✅ Checks antes de concluir | `npm run lint` → `npm run typecheck` → `npm run test` |
| ✅ Strict Mode | TypeScript configurado em `strict: true` e app envolvido em `<React.StrictMode>` |
| ✅ Tipagem estrita | Nenhuma variável ou retorno como `any`. Use `unknown` se estritamente necessário. |
| ✅ Separação UI/Lógica | Custom Hooks gerenciam estado complexo, componentes renderizam a UI. |
| ❌ Sem mutação direta | Sempre retorne novos objetos/arrays em atualizações de estado local ou global. |
| ❌ Sem "Prop Drilling" profundo | Use Zustand ou Context API quando passar de 2-3 níveis de profundidade. |
| ❌ Sem dependências vazando | Todo `useEffect`, `useMemo` ou `useCallback` deve ter array de dependências rigorosamente preenchido. |

## Comandos Essenciais

```bash
npm install      # Instalar dependências
npm run dev      # Iniciar servidor de desenvolvimento (Vite)
npm run build    # Compilar projeto para produção
npm run lint     # Rodar ESLint
npm run test     # Rodar suíte de testes (Vitest)
npm run typecheck# Checar tipos TypeScript
```

## Nomenclatura

| Tipo | Convenção | Exemplo |
|------|-----------|---------|
| Componentes | PascalCase | `UserProfile.tsx`, `SubmitButton.tsx` |
| Pastas de componentes | PascalCase (quando agrupa componente) ou kebab-case (padrão do time) | `UserProfile/` ou `user-profile/` |
| Custom Hooks | camelCase prefixado com 'use' | `useAuth.ts`, `useFetchData.ts` |
| Stores (Zustand) | camelCase prefixado com 'use' e sufixo 'Store' | `useUserStore.ts` |
| Arquivos utilitários | camelCase ou kebab-case | `formatDate.ts`, `api-client.ts` |
| Interfaces | PascalCase (prefixadas com 'I' é opcional, preferência sem prefixo) | `User`, `ApiResponse` |
| Props | Nomenclatura do componente + 'Props' | `UserProfileProps` |

## Anti-padrões

| Não faça | Faça |
|----------|------|
| Tipos `any` soltos | Tipos estritos (`interface`, `type`, `unknown`) |
| Lógica pesada em Componentes | Extrair para `src/hooks/` ou `src/utils/` |
| Fetching em `useEffect` direto | Use React Query (`useQuery`, `useMutation`) |
| Encadeamento de `useState` longo | Use `useReducer` ou Zustand (`src/store/`) |
| Classes CSS inline (`style={{}}`) | Tailwind CSS utility classes |
| Ignorar exaustão de dependências | Seguir os alertas do `eslint-plugin-react-hooks` |

## Guia de Estilo

- **Functional Components:** Use sempre componentes funcionais e hooks. Não use Class Components.
- **Early Returns:** Evite aninhamentos profundos. Valide erros e retorne precocemente.
- **Desestruturação:** Desestruture props e estados diretamente. Ex: `const { name, age } = props;`
- **Memoização:** Use `React.memo`, `useMemo` e `useCallback` quando for passar funções/objetos via props para evitar re-renders desnecessários.
- **Exportações:** Prefira "named exports" para funções/hooks e "default exports" apenas para componentes de página/rotas.

## Metodologia AI-SDD

O projeto segue **AI-Specification-Driven Development**:

- **Sistema:** `ai-sdd/system/` — vision, product_map, roadmap
- **Funcionalidades:** `ai-sdd/prd-[feature-slug]/` — PRD + design + techspec + tasks por funcionalidade
- **Commands:** `.opencode/commands/` — automação de planejamento (vision, product map, roadmap, PRD, design, techspec, tasks)
- **Agents:** `.opencode/agents/` — execução (`dev` implementa tarefas, `reviewer` revisa PRs)
- **Skills:** `.opencode/skills/` — padrões React (Componentes, State, Hooks, etc.) e procedimentos AI-SDD
- **Rules:** `.opencode/rules/` — convenções por camada (components, hooks, state-management, etc.)

Veja `.opencode/rules/` para convenções detalhadas por camada.
