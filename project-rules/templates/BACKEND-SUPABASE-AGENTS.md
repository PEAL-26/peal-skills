# Regras do Projeto (Backend - Supabase & PostgreSQL)

Este arquivo define as diretrizes para este projeto de backend com Supabase e Edge Functions.

## Perfil do Projeto
- **Tipo:** BACKEND
- **Stack:** Supabase (PostgreSQL + RLS + Edge Functions + Storage + Auth)
- **Linguagem:** SQL / TypeScript (Deno & Node)
- **Multi-Tenancy:** RLS com `organization_id` em todas as tabelas de negócio

---

## 📚 Referências Oficiais da Skill `project-rules`

As regras moram dentro da própria pasta da skill `project-rules`, sob `references/`. Os caminhos abaixo são relativos a essa pasta — carrega apenas os módulos que a tarefa pedir.

- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Backend Supabase:**
  - [Regras de Backend Supabase](../references/backend/supabase.md)

---

## 🛠️ Comandos Supabase
- Nova Migração: `supabase migration new <nome_da_migracao>`
- Iniciar Local: `supabase start`
- Testar Edge Functions: `supabase functions serve`
- Gerar Tipos TS: `supabase gen types typescript --local > src/types/database.types.ts`

---

## 🚫 O Que NUNCA Fazer
1. Nunca alterar arquivos de migração que já foram aplicados em produção.
2. Nunca desativar RLS ou criar políticas permissivas `USING (true)` para escrita.
3. Nunca armazenar um cliente Supabase autenticado em singleton global no servidor.
4. Nunca confiar em `raw_user_meta_data` para checagem de papéis ou privilégios de acesso.
