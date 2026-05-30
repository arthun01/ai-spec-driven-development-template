# Padrões de Testes React

## 1. Stack de Testes
Utilizamos **Vitest** como Test Runner (integrado ao Vite) e **React Testing Library (RTL)** para testar o comportamento dos componentes a partir da perspectiva do usuário.

## 2. Foco no Comportamento, Não na Implementação
Teste o que o componente **faz**, e não como ele foi implementado. Evite testar estados internos de um componente funcional, ou verificar se uma função foi instanciada. Teste os inputs (props e interações com usuário) e outputs (renderização no DOM).
```tsx
// ✅ Certo (testar via DOM/RTL)
render(<Counter />);
const button = screen.getByRole('button', { name: /incrementar/i });
fireEvent.click(button);
expect(screen.getByText('1')).toBeInTheDocument();
```

## 3. Consultas Acessíveis (Queries RTL)
Use a ordem de prioridade de queries recomendada pelo React Testing Library, focada em acessibilidade (`getByRole`, `getByLabelText`, `getByText`). Só utilize `getByTestId` como último recurso em cenários complexos de selecionar no DOM.

## 4. Mocks de API e Hooks
- Para testes de integração que necessitam de retornos de API remota, use bibliotecas como MSW (Mock Service Worker).
- Se estiver testando lógicas puras dentro de um custom hook, utilize a biblioteca `@testing-library/react-hooks` ou a funcionalidade embutida no RTL a partir da versão 13.1 `renderHook`.
- Evite "mockar" todos os componentes filhos; mantenha testes o mais próximos possíveis de como o componente roda integrado.

## 5. Setup / Teardown
O Vitest deve estar configurado para limpar automaticamente o DOM após cada teste. Mantenha os mocks isolados chamando `vi.clearAllMocks()` e garantindo que o estado de um teste não afete o outro.

## 6. Coverage
Testes devem cobrir os fluxos principais (Happy Path) e cenários críticos de erro (Edge Cases). O linting do projeto prevê cobertura, mantenha-a a níveis recomendados.
