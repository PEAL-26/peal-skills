# Fullstack Separado (Monorepo)

Diretrizes para arquiteturas Fullstack organizadas em monorepo com pacotes e aplicações desacopladas gerenciadas por pnpm workspaces ou Turborepo.

---

## 1. Topologia Recomendada de Monorepo

```text
meu-projeto/
├── apps/
│   ├── web/          # Aplicação Web (Next.js / Vite SPA)
│   ├── mobile/       # Aplicativo Móvel (Expo / React Native)
│   └── admin/        # Painel Administrativo interno
├── packages/
│   ├── backend/      # Orquestração server-only, workflows privilegiados, DB
│   ├── supabase/     # Cliente Supabase seguro (client-safe)
│   ├── schemas/      # Esquemas Zod compartilhados entre web, mobile e backend
│   ├── types/        # Interfaces e contratos TypeScript neutros de plataforma
│   └── utils/        # Funções utilitárias puras
└── package.json
```

---

## 2. Regras Rígidas de Fronteira entre Pacotes

1. **Neutralidade de Plataforma:**
   - `packages/types`, `packages/schemas` e `packages/utils` devem ser 100% neutros de plataforma.
   - **Nunca** importe bibliotecas web-only (como `next/navigation`, bibliotecas de DOM ou React Web) ou mobile-only dentro destes pacotes compartilhados.
2. **Pacote Client-Safe (`packages/supabase`):**
   - Este pacote pode ser consumido livremente por `apps/web` e `apps/mobile`.
   - **Nunca** importe código server-only ou variáveis de ambiente com credenciais privilegiadas (`service_role`, segredos de banco) para dentro deste pacote.
3. **Pacote Server-Only (`packages/backend`):**
   - É estritamente proibido importar `@meu-projeto/backend` a partir de `apps/mobile` ou de qualquer componente com `'use client'` em `apps/web`.
   - Apenas componentes de servidor (Server Components, Route Handlers) ou CLIs administrativas podem importar este pacote.
4. **Isolamento de Segredos de Ambiente:**
   - Nunca coloque chaves `service_role` ou segredos privados sob nomes como `NEXT_PUBLIC_*` ou `EXPO_PUBLIC_*`.

---

## 3. Gestão de Dependências e Build

- Todas as dependências compartilhadas e scripts de compilação devem ser orquestrados via `pnpm`:
  - `pnpm dev` — Inicia todas as aplicações em paralelo.
  - `pnpm build` — Compila pacotes dependentes antes de compilar as aplicações.
  - `pnpm -r typecheck` — Valida a tipagem estrita em todo o monorepo de uma só vez.
