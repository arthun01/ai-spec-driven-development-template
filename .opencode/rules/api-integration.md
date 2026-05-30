# Integração de APIs e Data Fetching

## 1. Padrão de Fetching (React Query)
Todo acesso a dados remotos do lado do cliente **DEVE** utilizar o React Query (`@tanstack/react-query`). Nunca faça chamadas diretas via `useEffect` puro na UI sem justificação forte.
- O React Query cuida de caching, invalidação, background re-fetching, e states de loading/error para nós.
```tsx
// ✅ Certo
export const useUsers = () => {
  return useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
  });
};
```

## 2. Separação de Serviços
Mantenha a camada HTTP (Axios/Fetch) separada da camada de Componentes/Hooks. Coloque as definições da API na pasta `src/services/`.
```tsx
// src/services/api/users.ts
import apiClient from '../apiClient';

export const fetchUsers = async (): Promise<User[]> => {
  const { data } = await apiClient.get('/users');
  return data;
};
```

## 3. Tratamento de Erros de API
Erros genéricos e interceptadores de autenticação devem ficar configurados diretamente no cliente da API (`axios.create` ou similar). Componentes cuidam de mostrar a interface apropriada.
- Sempre trate erros de tipagem com TypeScript (use tipos que modelam a resposta de erro da sua API para que o React Query repasse o tipo correto em `error`).

## 4. Mutations e Efeitos Colaterais
Para criar, atualizar ou deletar dados, use `useMutation`. Após a mutação, use o `QueryClient` para invalidar as queries relacionadas, mantendo o frontend em sincronia com o servidor automaticamente.
```tsx
// ✅ Certo
export const useCreateUser = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: createUserApi,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
};
```

## 5. Loading States e Skeletons
Assegure-se de que cada componente que depende de chamadas de API trate `isLoading` e `isError` (retornados pelo React Query). Mostre preferencialmente UI Skeletons em vez de "spinners" opacos para melhorar a percepção de performance.
