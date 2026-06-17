# Padrões de Testes React Native

## 1. Stack de Testes
Utilizamos **Jest** como Test Runner e **React Native Testing Library (RNTL)** para testar o comportamento dos componentes a partir da perspectiva do usuário.

## 2. Foco no Comportamento, Não na Implementação
Teste o que o componente **faz**, e não como ele foi implementado. Evite testar estados internos de um componente funcional, ou verificar se uma função foi instanciada. Teste os inputs (props e interações com usuário) e outputs (renderização).
```tsx
// ✅ Certo (testar via RNTL)
render(<Counter />);
const button = screen.getByRole('button', { name: /incrementar/i });
fireEvent.press(button);
expect(screen.getByText('1')).toBeTruthy();
```

## 3. Consultas Acessíveis (Queries RNTL)
Use a ordem de prioridade de queries recomendada pelo React Native Testing Library, focada em acessibilidade (`getByRole`, `getByLabelText`, `getByText`). Só utilize `getByTestId` como último recurso em cenários complexos.

## 4. Mocks de API e Hooks
- Para testes de integração que necessitam de retornos de API remota, use mocks ou ferramentas adequadas.
- Se estiver testando lógicas puras dentro de um custom hook, utilize `renderHook` do RNTL.
- Evite "mockar" todos os componentes filhos; mantenha testes o mais próximos possíveis de como o componente roda integrado.

## 5. Setup / Teardown
O Jest deve estar configurado corretamente para resetar mocks após cada teste. Mantenha os mocks isolados chamando `jest.clearAllMocks()` e garantindo que o estado de um teste não afete o outro.

## 6. Coverage
Testes devem cobrir os fluxos principais (Happy Path) e cenários críticos de erro (Edge Cases). O linting do projeto prevê cobertura, mantenha-a a níveis recomendados.
