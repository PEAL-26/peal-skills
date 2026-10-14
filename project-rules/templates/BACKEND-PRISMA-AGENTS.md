# Regras do Projeto (Backend - Prisma ORM & Node.js)

Este arquivo define as diretrizes para este projeto de backend utilizando Prisma ORM e Better Auth / Node.js.

## Perfil do Projeto
- **Tipo:** BACKEND
- **Stack:** Node.js + Prisma ORM + PostgreSQL
- **Autenticação:** Better Auth
- **Linguagem:** TypeScript Estrito (`strict: true`)

---

## 📚 Referências Oficiais da Skill `project-rules`

As regras moram dentro da própria pasta da skill `project-rules`, sob `references/`. Os caminhos abaixo são relativos a essa pasta — carrega apenas os módulos que a tarefa pedir.

- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Backend Prisma:**
  - [Regras de Backend Prisma ORM](../references/backend/prisma.md)

---

## 🛠️ Comandos Prisma
- Nova Migração: `pnpm prisma migrate dev --name <descricao>`
- Gerar Cliente: `pnpm prisma generate`
- Prisma Studio: `pnpm prisma studio`
- Typecheck: `npx tsc --noEmit`

---

## 🚫 O Que NUNCA Fazer
1. Nunca fazer queries ao banco via Prisma diretamente em componentes ou rotas públicas (use módulos em `src/modules/**`).
2. Nunca ativar `session.cookieCache` no Better Auth (garante que sessões revogadas percam acesso imediatamente).
3. Nunca importar o módulo de servidor `auth.server.ts` dentro do código do browser ou do cliente.
4. Nunca alterar migrações de produção diretamente no banco sem versionamento no Prisma.
