# Padrões de Hooks React

## 1. Custom Hooks Lógicos (`src/hooks/`)
Utilize Custom Hooks sempre que lógica complexa de estado, lifecycle ou integração precisar ser reutilizada ou apenas separada da camada de UI. Prefixá-los sempre com `use`.
```tsx
// ✅ Certo
export const useAuth = () => {
  const [user, setUser] = useState<User | null>(null);
  // ...
  return { user, login, logout };
}
```

## 2. Dependências em `useEffect`
As dependências do `useEffect` DEVEM ser declaradas exaustivamente. Não desative a regra `react-hooks/exhaustive-deps`. Se ocorrerem loops infinitos, revise o fluxo de dependências (geralmente falta memoizar um objeto ou função, ou a dependência não deveria estar lá e precisa de outro design).
```tsx
// ✅ Certo
useEffect(() => {
  fetchData(userId);
}, [userId, fetchData]); // fetchData deve estar em um useCallback
```

## 3. Otimização com `useMemo` e `useCallback`
- Use `useCallback` para funções que são passadas como props para componentes filhos memoizados (`React.memo`) ou que fazem parte da array de dependências de outros Hooks.
- Use `useMemo` para cálculos custosos computacionalmente ou para derivar estado garantindo a mesma referência de memória, evitando re-renders nas branches filhas.
```tsx
// ✅ Certo
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
const memoizedCallback = useCallback(() => doSomething(a, b), [a, b]);
```

## 4. Retornos de Hooks
Geralmente, prefira retornar objetos em hooks que exportam múltiplos valores ou funções (facilita acesso ordenado e a adição de novos retornos). Se o hook atuar como uma dupla de tuplas (similar a `useState`), retorne um array.
```tsx
// ✅ Certo
return { data, loading, error, refetch };
```

## 5. Cuidados com `useRef`
Use `useRef` para referências ao DOM ou variáveis mutáveis que **não devem disparar re-renderizações**. Nunca use para estado que afeta o que é exibido no JSX.
