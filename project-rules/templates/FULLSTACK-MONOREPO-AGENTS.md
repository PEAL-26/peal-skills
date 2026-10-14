# Regras do Projeto (Fullstack Separado - Monorepo)

Este arquivo define as diretrizes de monorepo e fronteiras entre pacotes carregadas automaticamente pelos assistentes de IA para este projeto.

## Perfil do Projeto
- **Tipo:** FULLSTACK (Separado em Monorepo)
- **Topologia:** `apps/` (web, mobile) + `packages/` (backend, supabase, schemas, types)
- **Gerenciador de Pacotes:** pnpm workspaces
- **Linguagem:** TypeScript Estrito (`strict: true`)
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
- **Monorepo e Fronteiras:**
  - [Fullstack Separado Monorepo](../references/fullstack/monorepo-separated.md)
- **Aplicações e Pacotes Específicos:**
  - Para Web (`apps/web`): consulte [Frontend Next.js](../references/frontend/nextjs-frontend.md)
  - Para Mobile (`apps/mobile`): consulte [Frontend Expo Mobile](../references/frontend/expo-mobile.md)
  - Para Backend (`packages/backend`): consulte [Backend Supabase](../references/backend/supabase.md) ou [Backend Prisma](../references/backend/prisma.md)

---

## 🛠️ Comandos de Monorepo
- Instalar: `pnpm install`
- Dev Geral: `pnpm dev`
- Typecheck Geral: `pnpm -r typecheck`
- Lint Geral: `pnpm -r lint`
- Formatação: `pnpm format`
- Build Geral: `pnpm build` (compila dependências internas na ordem correta)

---

## 🚫 O Que NUNCA Fazer
1. Nunca importar `@projeto/backend` em `apps/mobile` ou em Client Components em `apps/web`.
2. Nunca adicionar bibliotecas específicas de Web ou Mobile nos pacotes neutros (`types`, `schemas`, `utils`).
3. Nunca colocar `service_role` ou segredos privados em pacotes consumidos pelo cliente (`packages/supabase`).
4. Nunca ignorar dependências circulares entre pacotes.
