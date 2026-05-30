# Gestão de Estado

## 1. Estado Local vs Global
A regra de ouro é manter o estado o mais próximo possível de onde é utilizado.
- **Estado Local (`useState`, `useReducer`):** Use para estados de formulários, toggles de UI, modais, etc.
- **Estado Global (Zustand):** Use apenas quando o estado precisar ser compartilhado entre múltiplos componentes distantes na árvore (evitar prop drilling profundo) ou quando for dados persistentes de contexto (ex: Tema da UI, Usuário Autenticado).

## 2. Zustand para Estado Global
Utilizamos **Zustand** por ser performático e não exigir boilerplate excessivo como o Redux.
- Crie stores granulares baseadas em domínio (`useAuthStore`, `useUiStore`), evitando uma "monolitic store".
- Exporte o store a partir de `src/store/`.
```tsx
// ✅ Certo
import { create } from 'zustand';

interface UiState {
  sidebarOpen: boolean;
  toggleSidebar: () => void;
}

export const useUiStore = create<UiState>((set) => ({
  sidebarOpen: false,
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
}));
```

## 3. Derivação de Estado
Se uma variável pode ser calculada a partir de estados existentes ou props, **não a armazene no estado**. Compute-a on-the-fly durante a renderização (podendo usar `useMemo` se a derivação for cara).
```tsx
// ❌ Errado
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState(''); // Estado redundante

// ✅ Certo
const fullName = `${firstName} ${lastName}`;
```

## 4. Acesso ao Estado Global
Ao acessar o store do Zustand, utilize seletores granulares para evitar re-renderizações desnecessárias. O componente só irá re-renderizar se a parte selecionada mudar.
```tsx
// ❌ Errado (renderiza sempre que QUALQUER coisa no store mudar)
const store = useUiStore();
const sidebarOpen = store.sidebarOpen;

// ✅ Certo (renderiza APENAS se sidebarOpen mudar)
const sidebarOpen = useUiStore((state) => state.sidebarOpen);
```
