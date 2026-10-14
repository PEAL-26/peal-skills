# Regras do Projeto (Frontend - Vite + React SPA)

Este arquivo define as diretrizes para este projeto de frontend Single Page Application (SPA) com Vite e React.

## Perfil do Projeto
- **Tipo:** FRONTEND
- **Build Tool:** Vite
- **Framework:** React 18 / 19
- **Roteador:** TanStack Router ou React Router DOM v6
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Estilos:** Tailwind CSS + Radix/shadcn

---

## 📚 Referências Oficiais da Skill `project-rules`
- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Formatação e Linting](../references/general/formatting-linting.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Frontend SPA:**
  - [Frontend Vite SPA](../references/frontend/vite-spa.md)
  - [Arquitetura de Componentes Híbrida](../references/frontend/components.md)
  - [Formulários e Prevenção de Concorrência](../references/frontend/forms.md)

---

## 🛠️ Comandos Vite
- Dev: `pnpm dev`
- Testes: `pnpm test`
- Typecheck: `npx tsc --noEmit`
- Lint: `pnpm lint`
- Build: `pnpm build`
- Preview: `pnpm preview`

---

## 🚫 O Que NUNCA Fazer
1. Nunca colocar chaves secretas ou credenciais de banco sob o prefixo `VITE_` (ficam embutidas no bundle do navegador).
2. Nunca fazer chamadas diretas de API nos componentes sem passar pela camada `services/` ou hooks do React Query.
3. Nunca declarar funções auxiliares inline dentro de componentes se tiverem mais de 10 linhas.
