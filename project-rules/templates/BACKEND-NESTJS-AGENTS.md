# Regras do Projeto (Backend - NestJS)

Este arquivo define as diretrizes para este projeto de backend com NestJS.

## Perfil do Projeto
- **Tipo:** BACKEND
- **Framework:** NestJS
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Arquitetura:** Modular em Camadas (Modules, Controllers, Services, Repositories)

---

## 📚 Referências Oficiais da Skill `project-rules`
- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Backend NestJS:**
  - [Regras de Backend NestJS](../references/backend/nestjs.md)

---

## 🛠️ Comandos NestJS
- Dev: `npm run start:dev` / `pnpm start:dev`
- Testes Unitários: `npm run test`
- Testes E2E: `npm run test:e2e`
- Lint: `npm run lint`
- Build: `npm run build`

---

## 🚫 O Que NUNCA Fazer
1. Nunca instanciar serviços ou repositórios diretamente com `new MyService()` (utilize Dependency Injection).
2. Nunca processar requisições sem DTOs validados via `ValidationPipe` estrito.
3. Nunca devolver exceções de banco cruas para o cliente (utilize o ExceptionFilter global `ActionResult`).
4. Nunca ignorar a validação do tenant/organizationId em queries de repositório.
