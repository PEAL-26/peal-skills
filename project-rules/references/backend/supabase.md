# Backend Supabase (PostgreSQL, RLS e Edge Functions)

Regras arquiteturais e padrões de segurança para bancos de dados PostgreSQL hospedados no Supabase, funções serverless e autenticação.

---

## 1. Banco de Dados e Migrações

- **Migrações Imutáveis:** O banco de dados é versionado exclusivamente via migrações SQL na pasta `supabase/migrations/` (geradas via `supabase migration new`).
  - **Nunca** altere arquivos de migração que já foram aplicados em produção. Qualquer alteração de esquema requer uma nova migração incremental.
- **Nomenclatura do Banco:** Tabelas e colunas devem utilizar **inglês e `snake_case`**.
  - Chaves primárias: `id UUID PRIMARY KEY DEFAULT gen_random_uuid()` (evite extensões legacy como `uuid-ossp`).
  - Timestamps padrão: `created_at TIMESTAMPTZ DEFAULT now() NOT NULL` e `updated_at TIMESTAMPTZ DEFAULT now() NOT NULL` (com trigger automático de atualização).
- **Soft Delete:** Para entidades que possuem histórico ou risco de exclusão acidental, adicione `deleted_at TIMESTAMPTZ DEFAULT NULL`. As políticas RLS e queries devem filtrar `deleted_at IS NULL`.

---

## 2. Row Level Security (RLS) e Multi-Tenancy

- **Deny-by-Default:** Toda tabela pública possui RLS ativado: `ALTER TABLE customers ENABLE ROW LEVEL SECURITY;`. Ninguém lê ou escreve sem uma política explícita.
- **Otimização de Performance no RLS:** Ao verificar o usuário autenticado na política RLS, sempre envolva `auth.uid()` em um subselect `(select auth.uid())`. Isso faz o PostgreSQL avaliar o ID do usuário uma única vez por consulta, em vez de reavaliar por linha:
  ```sql
  -- ✅ Rápido e otimizado para o planejador do Postgres
  CREATE POLICY "Users can view tenant customers"
  ON customers FOR SELECT
  TO authenticated
  USING (organization_id = (SELECT (auth.jwt() -> 'app_metadata' ->> 'organization_id')::uuid));
  ```
- **Nunca `USING (true)`:** É terminantemente proibido criar políticas `USING (true)` para operações de escrita em tabelas autenticadas.

---

## 3. Clientes Supabase no Servidor (`@supabase/ssr`)

- **Instâncias com Escopo de Requisição:** Ao utilizar Server Components, Route Handlers ou Server Actions, crie instâncias de cliente com `@supabase/ssr` passando os cookies da requisição atual.
- **Proibição de Singleton Autenticado:** **Nunca** armazene um cliente Supabase autenticado em uma variável global singleton no servidor. Isso causaria vazamento de sessão entre requisições de usuários diferentes.
- **`getUser()` vs `getSession()`:** Nunca confie em `supabase.auth.getSession()` para autorização no servidor (o token pode ter sido revogado). Utilize sempre `await supabase.auth.getUser()`, que valida a sessão contra o servidor do Supabase.

---

## 4. Edge Functions (Deno / TypeScript)

- **Arquitetura em Camadas:**
  ```text
  supabase/functions/
  ├── _shared/         # Módulos compartilhados (runtime.ts, types.ts, cors.ts)
  └── scan-emails/
      ├── handler.ts   # Entrada HTTP (Web API Request/Response, status codes)
      ├── service.ts   # Orquestração da lógica de negócio
      └── index.ts     # Deno.serve() conectando o handler
  ```
- **Segurança de Webhooks e Chaves:**
  - Valide headers de autenticação (ex: `Authorization: Bearer <CRON_SECRET>` ou segredos de webhook de terceiros).
  - Senhas de e-mail (IMAP) e credenciais de provedores externos devem ser criptografadas em repouso com AES-256-GCM antes de persistir no PostgreSQL.
