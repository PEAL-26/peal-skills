# Frontend SPA (Vite + React)

Diretrizes para aplicações Single Page Application (SPA) utilizando Vite, React 18/19 e TypeScript.

---

## 1. Estrutura de Diretórios Recomendada

```text
src/
├── assets/          # Imagens, vetores e fontes estáticas
├── components/      # Componentes UI (ui/ atômicos e shared/ compostos)
├── config/          # Configurações globais e variáveis de ambiente
├── hooks/           # Custom hooks de lógica e data fetching
├── lib/             # Clientes de terceiros (supabase, axios, etc.)
├── pages/           # Componentes de páginas do roteador
├── services/        # Camada de abstração de chamadas à API
├── types/           # Interfaces e tipos TypeScript compartilhados
└── utils/           # Funções utilitárias puras
```

---

## 2. Variáveis de Ambiente no Vite

- **Prefixo Obrigatório:** Apenas variáveis com o prefixo `VITE_` são expostas ao cliente via `import.meta.env`.
- **Tipagem Obrigatória:** Declare todas as variáveis de ambiente na interface `ImportMetaEnv` dentro de `src/vite-env.d.ts`:
  ```typescript
  /// <reference types="vite/client" />

  interface ImportMetaEnv {
    readonly VITE_API_URL: string;
    readonly VITE_SUPABASE_URL: string;
    readonly VITE_SUPABASE_ANON_KEY: string;
  }

  interface ImportMeta {
    readonly env: ImportMetaEnv;
  }
  ```
- **Segurança Absoluta:** Nunca exponha tokens secretos, senhas de banco ou chaves administrativas no `VITE_`. Tudo no bundle do Vite é público e visível no inspecionador de código do navegador.

---

## 3. Roteamento e Data Fetching

- **Roteamento:** Utilize TanStack Router ou React Router DOM v6+.
- **Data Fetching:** Abstraia todas as chamadas de API em uma camada `src/services/`. Nunca faça chamadas diretas com `fetch` ou `axios` dentro de componentes React; utilize React Query (TanStack Query) encapsulado em custom hooks na pasta `src/hooks/`.
- **Testes:** Utilize Vitest para testes unitários e de integração, aproveitando a mesma configuração do `vite.config.ts`.
