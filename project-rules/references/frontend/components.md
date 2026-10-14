# Arquitetura de Componentes React (Padrão Híbrido)

Este documento define o padrão oficial de estruturação, componentização e otimização de componentes React.

---

## 1. Padrão Híbrido de Estrutura de Arquivos

Adotamos uma abordagem proporcional à complexidade do componente:

### A. Componentes Complexos, com Formulários ou Múltiplos Estados
Componentes com lógica densa, validações ou integração de dados residem em uma **pasta dedicada**, isolando a apresentação visual da lógica de negócio:

```text
src/components/shared/user-profile/
├── user-profile.tsx   # Componente visual puro (apenas renderização de JSX)
├── use-logic.ts       # Custom hook contendo toda a lógica, estados e handlers
├── types.ts           # Props do componente, interfaces locais e constantes
└── index.ts           # Barrel exportando o componente e tipos
```

#### Exemplo de Implementação:

**`types.ts`:**
```typescript
export interface UserProfileProps {
  userId: string;
  onUpdateSuccess?: () => void;
}

export interface UserFormState {
  fullName: string;
  bio: string;
}
```

**`use-logic.ts`:**
```typescript
import { useState } from 'react';
import { UserProfileProps } from './types';

export function useLogic(props: UserProfileProps) {
  const [loading, setLoading] = useState(false);

  const handleSubmit = async (data: any) => {
    setLoading(true);
    // Lógica assíncrona
    setLoading(false);
    props.onUpdateSuccess?.();
  };

  return {
    loading,
    handleSubmit,
  };
}
```

**`user-profile.tsx`:**
```tsx
import { memo } from 'react';
import { UserProfileProps } from './types';
import { useLogic } from './use-logic';

function UserProfileComponent(props: UserProfileProps) {
  const { loading, handleSubmit } = useLogic(props);

  return (
    <div className="p-4 bg-surface-primary rounded-lg">
      {/* Apenas renderização JSX limpa */}
    </div>
  );
}

export const UserProfile = memo(UserProfileComponent);
```

**`index.ts`:**
```typescript
export { UserProfile } from './user-profile';
export type { UserProfileProps } from './types';
```

---

### B. Componentes Visuais, Atômicos ou Primitivos
Componentes simples sem lógica pesada (como botões customizados, badges, cards informativos) devem ser mantidos como **arquivo único** em `kebab-case.tsx` para evitar excesso de pastas e barrels:

```text
src/components/ui/status-badge.tsx
src/components/shared/stat-card.tsx
```

---

## 2. Regras Essenciais de Componentização

1. **Nunca Declarar Componentes Dentro de Componentes:**
   - Declarar uma função de componente dentro do corpo de outro componente recria a definição a cada render, destruindo o estado interno dos filhos e degradando performance.
   - ❌ **Proibido:**
     ```tsx
     function Parent() {
       const Child = () => <div>...</div>; // RECRIA A CADA RENDER!
       return <Child />;
     }
     ```
   - ✅ **Correto:** Extraia `Child` para fora da função ou para um arquivo próprio.

2. **Proibido Funções Auxiliares que Retornam JSX no Corpo:**
   - Funções como `renderItem()` ou `getStatusBadge()` dentro de um componente são anti-patterns. Devem ser extraídas como componentes dedicados com props tipadas.

3. **Critérios Obrigatórios de Extração:**
   - Extraia para componente dedicado sempre que o JSX condicional encadear **3 ou mais ternários** (`a ? b ? c : d : e`) ou aninhar condições a **mais de 2 níveis**.
   - Extraia blocos repetidos em `.map()` que possuam mais de 5 linhas ou estado próprio.

4. **Memoização Responsável (`memo`):**
   - Exporte componentes Client Components (`'use client'`) com props envolvidos em `memo`: `export const MyComponent = memo(MyComponentInner);`.
   - **Estabilização de Props:** Props passadas para componentes memoizados devem ser referencialmente estáveis:
     - Handlers de evento via `useCallback`.
     - Objetos ou arrays estáticos definidos em constantes de nível de módulo (fora do componente).
     - Nunca passe arrow functions ou objetos criados inline no render diretamente para componentes memoizados.
