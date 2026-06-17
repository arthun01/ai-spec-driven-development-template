# Padrões de Componentes React Native

## 1. Functional Components
Sempre utilize Functional Components. Class Components estão obsoletos e não devem ser utilizados na base de código atual.
```tsx
// ✅ Certo
export const UserProfile = () => { ... }

// ❌ Errado
export class UserProfile extends React.Component { ... }
```

## 2. Tipagem Rigorosa de Props
Todo componente que recebe propriedades deve ter uma interface `Props` devidamente definida. Nunca use `any`.
```tsx
// ✅ Certo
interface UserProfileProps {
  userId: string;
  onUpdate: (data: UserData) => void;
  isActive?: boolean;
}

export const UserProfile = ({ userId, onUpdate, isActive = false }: UserProfileProps) => { ... }
```

## 3. Desestruturação de Props
Sempre desestruture as props diretamente na declaração dos parâmetros do componente. Isso melhora a legibilidade.
```tsx
// ✅ Certo
export const Card = ({ title, children }: CardProps) => { ... }

// ❌ Errado
export const Card = (props: CardProps) => { ... props.title ... }
```

## 4. Separação de UI e Lógica
Componentes de UI devem focar em renderização. Se um componente acumular muita lógica de negócios, cálculos ou múltiplos `useEffect`, extraia essa lógica para um Custom Hook.
```tsx
// ✅ Certo
export const UserDashboard = () => {
  const { user, loading, error, refresh } = useUserDashboard();
  
  if (loading) return <ActivityIndicator />;
  if (error) return <Text>{error.message}</Text>;
  
  return <UserView user={user} onRefresh={refresh} />;
}
```

## 5. Early Returns
Evite aninhar condições JSX (`if/else` longos ou múltiplos ternários). Valide falhas, loading states e erros no topo do componente e faça um "early return".
```tsx
// ✅ Certo
if (!isAuthenticated) return <Redirect to="/login" />; // Ou navigate para React Navigation
if (isLoading) return <ActivityIndicator />;
return <MainContent />;
```

## 6. Otimização de Renderização
Para componentes pesados que recebem props primitivas ou callbacks cacheados, considere o uso de `React.memo` para evitar re-renderizações desnecessárias. Apenas aplique se o custo de renderização for comprovadamente alto.
