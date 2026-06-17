---
paths:
  - "src/**/*.ts"
  - "src/**/*.tsx"
---

# Guia de Estilo React & TypeScript

- **Nomenclatura:** Utilize PascalCase para Componentes e interfaces (`UserProfile`, `UserProps`), e camelCase para variáveis, funções e Custom Hooks (`useAuth`, `formatDate`).
- **Tipagem:** Utilize tipagem estrita no TypeScript. Evite o uso de `any`. Prefira `unknown` caso o tipo exato não seja conhecido de antemão.
- **Exportações:** Prefira "named exports" para funções e hooks utilitários. Use "default exports" apenas para componentes de página/rotas, de acordo com o padrão do framework (como Next.js ou React Router).
- **Componentes:** Sempre escreva componentes funcionais. Componentes de classe não devem ser utilizados.
- **Imports:** Mantenha a ordem de importações: 1. Bibliotecas externas (ex: `react`, `zustand`), 2. Componentes internos absolutos, 3. Imports relativos locais.
- **Desestruturação:** Sempre desestruture props nos parâmetros da função do componente: `const Card = ({ title, children }: CardProps) => { ... }`.
- **CSS e Estilização:** Utilize classes utilitárias do NativeWind (Tailwind CSS para React Native) ou `StyleSheet.create`. Evite estilos inline (`style={{...}}`) a menos que seja um valor estritamente dinâmico (como cálculos de dimensões via JS).
