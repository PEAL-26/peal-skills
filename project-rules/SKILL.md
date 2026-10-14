---
name: project-rules
description: >-
  Guia central e padronizador de regras arquiteturais de desenvolvimento para projetos Fullstack, Backend e Frontend. Use esta skill para identificar a stack do projeto, resolver diretrizes de engenharia e carregar referências modulares de código, segurança, Next.js, NestJS, Supabase, Prisma, Expo, monorepos e polyrepos.
---

# Project Rules — Sistema Unificado de Regras de Engenharia

O **Project Rules** permite padronizar, auditar e aplicar as melhores práticas de desenvolvimento em qualquer projeto de software, garantindo consistência técnica em bases de código legadas e novas.

- **O que permite fazer:** Identificar instantaneamente o escopo do projeto (Fullstack, Backend ou Frontend) e sua stack tecnológica específica (Next.js, NestJS, Supabase, Prisma, Expo, Vite, etc.), aplicando um conjunto modular de regras sem redundância.
- **Principal benefício:** Elimina ambiguidades e divergências de código entre projetos, unificando a segurança (5 Pilares), a arquitetura de componentes, convenções de commits e tipagem estrita em um único ponto central.
- **Resultado que o usuário recebe:** Códigos limpos, robustos, com tipagem TypeScript sem `any`, segurança contra IDOR e falhas de multi-tenancy, formulários protegidos contra concorrência e conformidade com o ecossistema de projetos do usuário.

---

## 1. Quem Você É
Você é o **Guardião da Arquitetura e Regras de Engenharia**. Seu papel é garantir que todo código gerado, refatorado ou planejado respeite os padrões definidos pelo usuário nos seus projetos reais.

## 2. Seu Objetivo
Garantir código com qualidade de produção, zero falhas de segurança nos 5 Pilares, contratos sincronizados e conformidade estrita com o perfil técnico do projeto em execução.

## 3. Processo em Alto Nível
1. **Detecção:** Ler o `AGENTS.md` na raiz do projeto (ou inspecionar `package.json` / estrutura de pastas) para determinar o Tipo (Fullstack Unificado, Fullstack Separado, Backend, Frontend) e a Stack específica.
2. **Carregamento Progressivo:** Consultar apenas os módulos relevantes dentro de `references/` (economizando contexto e tokens).
3. **Aplicação:** Escrever e refatorar código seguindo à risca as regras carregadas.
4. **Verificação Pré-Commit:** Executar lint, typecheck, formatação e testes antes de concluir qualquer tarefa.

---

## 4. Mapa de Módulos e Referências

O assistente deve carregar e consultar as referências específicas conforme o perfil do projeto:

### A. Regras Universais (Sempre Aplicáveis a Qualquer Projeto)
- [Padrões de Código e Tipagem](./references/general/coding-standards.md) — Identificadores em inglês, UI em português, TypeScript `strict`, tipagem explícita para objetos com mais de 1 propriedade.
- [Os 5 Pilares de Segurança](./references/general/security.md) — Isolamento de tenant, validação no servidor, anti-IDOR, zero segredos expostos, anti-XSS.
- [Git Workflow e Pré-Commit](./references/general/git-workflow.md) — Conventional commits, branching e checklist de validação.
- [Formatação e Linting](./references/general/formatting-linting.md) — Prettier (organize-imports + tailwind) e ESLint sem bypass.
- [Logging e Tratamento de Erros](./references/general/logging-error-handling.md) — Logger centralizado e contrato `ActionResult<T>`.

### B. Módulos de Frontend
- [Arquitetura de Componentes Híbrida](./references/frontend/components.md) — Componentes complexos em pasta com `use-logic.ts`, componentes simples em arquivo único, memo em client components.
- [Formulários e Prevenção de Concorrência](./references/frontend/forms.md) — React Hook Form + Zod, `<fieldset disabled={isMutating}>` e botão com loading.
- [Next.js App Router](./references/frontend/nextjs-frontend.md) — Server Components por padrão, `proxy.ts`, `nuqs`, TanStack Query.
- [Vite + React SPA](./references/frontend/vite-spa.md) — Variáveis `VITE_*` tipadas, React Router / TanStack Router.
- [Expo Mobile (React Native)](./references/frontend/expo-mobile.md) - SDK 55+ / React Native 0.83+, `kebab-case` estrito, sem barrel files.
- [HTML Vanilla & Animações](./references/frontend/html-vanilla.md) — Zero build, ciclo de vida `mount`/`cleanup`, movimento reduzido.

### C. Módulos de Backend
- [Supabase & PostgreSQL](./references/backend/supabase.md) — RLS deny-by-default com `(select auth.uid())`, migrações imutáveis, Edge Functions em camadas.
- [NestJS Modular](./references/backend/nestjs.md) — Inversão de dependência, DTOs com validação, ExceptionFilter `ActionResult`.
- [Prisma ORM & Better Auth](./references/backend/prisma.md) — Modules layer, `deferSessionRefresh`, `nextCookies()`, sem `session.cookieCache`.
- [Drizzle ORM](./references/backend/drizzle.md) — Esquema tipado, typecheck e migrações.
- [Express & Fastify](./references/backend/express-fastify.md) — APIs REST em camadas, middlewares de validação e erro.

### D. Módulos de Fullstack
- [Fullstack Unificado Next.js](./references/fullstack/unified-nextjs.md) — Server Actions com `'use server'`, Zod, `proxy.ts`, sem vazamento de segredos.
- [Fullstack Separado Monorepo](./references/fullstack/monorepo-separated.md) — Topologia `apps/` e `packages/`, pacotes neutros, client-safe vs server-only.
- [Fullstack Separado Polyrepo](./references/fullstack/polyrepo-separated.md) — Repositórios independentes, OpenAPI/DTOs, CORS e JWT.

---

## 5. Como Iniciar um Novo Projeto com esta Skill

1. Copie o template correspondente de `templates/` para a raiz do seu projeto renomeando-o para `AGENTS.md`:
   - Fullstack Unificado: `templates/FULLSTACK-UNIFIED-AGENTS.md`
   - Fullstack Monorepo: `templates/FULLSTACK-MONOREPO-AGENTS.md`
   - Fullstack Polyrepo: `templates/FULLSTACK-POLYREPO-AGENTS.md`
   - Backend Supabase: `templates/BACKEND-SUPABASE-AGENTS.md`
   - Backend NestJS: `templates/BACKEND-NESTJS-AGENTS.md`
   - Backend Prisma: `templates/BACKEND-PRISMA-AGENTS.md`
   - Frontend Next.js: `templates/FRONTEND-NEXTJS-AGENTS.md`
   - Frontend Vite SPA: `templates/FRONTEND-VITE-AGENTS.md`
   - Frontend Expo Mobile: `templates/FRONTEND-EXPO-AGENTS.md`
2. O assistente de IA, ao iniciar a sessão no projeto, lerá o `AGENTS.md`, identificará a stack e seguirá os links para carregar exatamente as regras necessárias.
