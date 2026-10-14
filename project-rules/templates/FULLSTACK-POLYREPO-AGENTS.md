# Regras do Projeto (Fullstack Separado - Polyrepo)

Este arquivo define as diretrizes para este repositório desacoplado, focado em contratos de API e comunicação de rede com o serviço complementar.

## Perfil do Projeto
- **Tipo:** FULLSTACK (Polyrepo / Repositório Independente)
- **Papel deste Repositório:** [ ] Frontend [ ] Backend
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Protocolo de Comunicação:** REST / JSON (ou WebSocket)

---

## 📚 Referências Oficiais da Skill `project-rules`
- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Formatação e Linting](../references/general/formatting-linting.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Polyrepo:**
  - [Fullstack Separado Polyrepo](../references/fullstack/polyrepo-separated.md)
- **Stack Específica deste Repositório:**
  - Se for Frontend: consulte referências em `references/frontend/`
  - Se for Backend: consulte referências em `references/backend/`

---

## 🛠️ Comandos de Desenvolvimento
- Dev: `npm run dev` / `pnpm dev`
- Testes: `npm test` / `pnpm test`
- Typecheck: `npx tsc --noEmit`
- Lint: `npm run lint` / `pnpm lint`
- Build: `npm run build`

---

## 🚫 O Que NUNCA Fazer
1. Nunca alterar um endpoint no backend ou payload de resposta sem atualizar o contrato de API.
2. Nunca hardcodear URLs como `http://localhost:3000` no frontend (use variáveis de ambiente).
3. Nunca abrir CORS com `Access-Control-Allow-Origin: *` em rotas que recebem cookies ou autenticação.
