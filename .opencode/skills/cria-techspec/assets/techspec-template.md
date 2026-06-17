# Tech Spec - [Nome da Funcionalidade]

## Resumo Executivo

[Breve visão técnica da abordagem de solução em 1-2 parágrafos:
- Decisões arquiteturais principais
- Estratégia de implementação
- Referência ao PRD correspondente]

## Arquitetura do Sistema

### Visão Geral dos Componentes

[Descrição dos componentes principais e suas responsabilidades:

- **[Componente 1]**: [Responsabilidade] — Novo / Modificado
- **[Componente 2]**: [Responsabilidade] — Novo / Modificado

Inclua TODOS os componentes novos ou que serão modificados.

Relacionamentos principais entre componentes e visão geral do fluxo de dados.]

## Design de Implementação

### Interfaces Principais

[Defina interfaces TypeScript principais (≤20 linhas por exemplo):

```typescript
// Exemplo
export interface UserData {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
}
```
]

### Árvore de Componentes (Component Tree)

[Defina a hierarquia visual e de componentes:

- **Componentes de Página**: Quais componentes roteáveis serão afetados
- **Componentes de UI**: Novos elementos de interface a criar
- **Hooks Customizados**: Lógica de interface encapsulada

Exemplo:
```text
DashboardPage
├── Sidebar
├── Header
└── MainContent
    ├── UserStats (usa useUserStats)
    └── RecentActivity
```
]

### Gestão de Estado (State Requirements)

[Defina os requisitos de estado para a funcionalidade:

- **Estado Local**: O que será mantido em `useState`/`useReducer`
- **Estado Global**: O que será mantido no `Zustand`
- **Server State**: O que será buscado/cacheado pelo `React Native Query`

Exemplo:
- Estado do modal de edição: Local (`isOpen`, `setIsOpen`)
- Sessão do usuário logado: Global (`useAuthStore`)
]

### Endpoints de API a Consumir

[Liste os endpoints que a UI precisará chamar (via Axios/Fetch + React Native Query):

| Método | Endpoint | Hook/Função | Descrição |
|--------|----------|-------------|-----------|
| GET | `/api/v1/users` | `useUsers()` | Listagem de usuários |
| POST | `/api/v1/users` | `useCreateUser()` | Criação de usuário |

Referencie requisitos do PRD (RF-XXX) quando aplicável.]

## Pontos de Integração

[Inclua apenas se a funcionalidade requer integrações externas:

- **Serviço/API**: [Nome] — [Propósito]
- **Autenticação**: [Método de autenticação enviado nos headers]
- **Tratamento de Erros**: [Abordagem para falhas e retries]
- **Fallback**: [Comportamento quando integração está indisponível na UI]]

## Abordagem de Testes

### Testes Unitários e Componentes

[Estratégia de testes com Expost + RTL:

- **Componentes**: Comportamento visual, acessibilidade (queries RTL), interações (user-event)
- **Hooks**: Lógica isolada (renderHook)
- **Utils**: Funções puras
- **Cenários críticos**: [Liste os mais importantes]]

### Mocks e Integração

[Testes de integração:

- **Mocks de API**: Interceptação de requests (ex: MSW) para simular o backend
- **Fluxos**: Testando um conjunto de componentes integrados
- **Dados de teste**: Factories ou dados estáticos necessários]

## Sequenciamento de Desenvolvimento

### Ordem de Construção

[Defina sequência de implementação respeitando dependências:

1. **[Interfaces/Tipos]**: [Justificativa] — Ref: RF-XXX
2. **[Serviços/Hooks de API]**: [Dependências]
3. **[Componentes Base]**: [O que precisa estar pronto antes]
4. **[Integração e Testes]**: [Validação final]]

### Dependências Técnicas

[Liste dependências bloqueantes:

- **Bibliotecas npm**: [Nome] — [Versão] — [Propósito]
- **Infraestrutura**: [O que precisa estar configurado]
- **Serviços externos/APIs**: [Disponibilidade requerida do Backend]]

## Monitoramento e Observabilidade

[Defina abordagem de monitoramento:

- **Logs**: Eventos principais a registrar no client-side
- **Métricas/Analytics**: Interações a serem trackeadas
- **Alertas**: Captura de exceções globais (ex: Sentry)]

## Considerações Técnicas

### Decisões Principais

[Documente decisões técnicas importantes:

| Decisão | Escolha | Justificativa | Alternativas Rejeitadas |
|---------|---------|---------------|------------------------|
| [Área] | [Escolha] | [Por quê] | [O que foi considerado] |
]

### Riscos Conhecidos

[Identifique riscos técnicos:

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------| 
| [Risco] | Alta/Média/Baixa | Alto/Médio/Baixo | [Ação] |
]

### Conformidade com Standards do Projeto

[Skills e padrões do projeto que se aplicam a esta spec:

- **[Skill/Padrão]**: [Como esta spec está conforme] ou [Desvio: justificativa e alternativa]
- **AGENTS.md**: [Conformidade com convenções React Native, TypeScript, Hooks, etc.]]

### Arquivos Relevantes e Dependentes

[Arquivos existentes que serão impactados ou consultados:

- `[caminho/arquivo]` — [Tipo de impacto: novo, modificado, consultado]
- `[caminho/arquivo]` — [Tipo de impacto]]
