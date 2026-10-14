# Regras do Projeto (Frontend - Next.js App Router)

Este arquivo define as diretrizes para este projeto de frontend com Next.js e React.

## Perfil do Projeto
- **Tipo:** FRONTEND
- **Framework:** Next.js 16+ (App Router) + React 19
- **Estilos:** Tailwind CSS v4 + shadcn/ui
- **Estado de URL:** nuqs
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Idioma do Código:** Inglês
- **Idioma da Interface:** Português (pt-AO / pt)

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

---

## 🛠️ Comandos Next.js
- Dev: `pnpm dev`
- Testes: `pnpm test`
- Typecheck: `npx tsc --noEmit`
- Lint: `pnpm lint`
- Formatação: `pnpm format`
- Build: `pnpm build`

---

## 🚫 O Que NUNCA Fazer
1. Nunca declarar componentes dentro do corpo de outros componentes.
2. Nunca esquecer de envolver componentes que leem `useSearchParams` / `nuqs` em `<Suspense>`.
3. Nunca aplicar `'use client'` em layouts inteiros; mantenha o layout como Server Component e isole o cliente em componentes filhos.
4. Nunca renderizar valores crus de enum diretamente para o usuário (use maps de tradução com fallback).
