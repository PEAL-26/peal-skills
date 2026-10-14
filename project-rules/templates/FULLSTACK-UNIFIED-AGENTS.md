# Regras do Projeto (Fullstack Unificado - Next.js)

Este arquivo define as diretrizes arquiteturais e regras de desenvolvimento carregadas automaticamente pelos assistentes de IA para este projeto.

## Perfil do Projeto
- **Tipo:** FULLSTACK (Unificado Monolítico)
- **Framework Principal:** Next.js 16+ (App Router) + React 19
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Estilos:** Tailwind CSS v4 + shadcn/ui
- **Idioma do Código:** Inglês
- **Idioma da Interface (UI):** Português (pt-AO / pt)

---

## 📚 Referências Oficiais da Skill `project-rules`

As regras moram dentro da própria pasta da skill `project-rules`, sob `references/`. Os caminhos abaixo são relativos a essa pasta — carrega apenas os módulos que a tarefa pedir.

- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Formatação e Linting](../references/general/formatting-linting.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Frontend:**
  - [Frontend Next.js App Router](../references/frontend/nextjs-frontend.md)
  - [Arquitetura de Componentes Híbrida](../references/frontend/components.md)
  - [Formulários e Prevenção de Concorrência](../references/frontend/forms.md)
- **Fullstack:**
  - [Fullstack Unificado Next.js](../references/fullstack/unified-nextjs.md)
- **Backend (Selecione o aplicável ao projeto):**
  - [Backend Supabase](../references/backend/supabase.md)
  - [Backend Prisma ORM](../references/backend/prisma.md)

---

## 🛠️ Comandos de Desenvolvimento
- Dev: `pnpm dev`
- Testes: `pnpm test`
- Typecheck: `npx tsc --noEmit`
- Lint: `pnpm lint`
- Formatação: `pnpm format`
- Build: `pnpm build` (sem timeout)

---

## 🚫 O Que NUNCA Fazer
1. Nunca colocar credenciais privadas ou `service_role` sob o prefixo `NEXT_PUBLIC_`.
2. Nunca importar módulos de banco de dados diretamente em componentes com `'use client'`.
3. Nunca declarar componentes ou funções que retornam JSX dentro do corpo de outro componente.
4. Nunca ignorar erros de build ou lint usando flags permissivas no `next.config.ts`.
5. Nunca alterar migrações de banco que já foram aplicadas em produção.
