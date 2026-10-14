# Backend Drizzle ORM

Diretrizes para modelagem type-safe e gerenciamento de banco de dados com Drizzle ORM e PostgreSQL.

---

## 1. Organização do Esquema de Banco de Dados

Em projetos modulares ou monorepos, o esquema do Drizzle deve residir em um pacote dedicado (ex: `packages/db/`):

```text
packages/db/
├── src/
│   ├── schema/
│   │   ├── users.ts
│   │   ├── organizations.ts
│   │   ├── audit-logs.ts
│   │   └── index.ts       # Re-exporta todas as tabelas e relações
│   ├── client.ts          # Instância tipada do cliente Drizzle
│   └── index.ts
├── drizzle.config.ts
└── migrations/
```

---

## 2. Padrões de Tabelas com Drizzle (`pgTable`)

```typescript
import { pgTable, uuid, varchar, timestamp } from 'drizzle-orm/pg-core';

export const organizations = pgTable('organizations', {
  id: uuid('id').primaryKey().defaultRandom(),
  name: varchar('name', { length: 255 }).notNull(),
  slug: varchar('slug', { length: 100 }).notNull().unique(),
  createdAt: timestamp('created_at', { withTimezone: true }).defaultNow().notNull(),
  updatedAt: timestamp('updated_at', { withTimezone: true }).defaultNow().notNull(),
});
```

- **Sincronização de Contratos:** Ao alterar o esquema de uma tabela:
  1. Atualize o arquivo em `schema/*.ts`.
  2. Garanta que a nova tabela está exportada em `schema/index.ts`.
  3. Gere a migração correspondente: `pnpm db:generate`.
  4. Valide a compilação completa: `pnpm -r typecheck`.

---

## 3. Consultas e Isolamento

- Utilize a Drizzle Relational Queries API (`db.query.users.findMany(...)`) para consultas expressivas com relações carregadas.
- Sempre envolva consultas em funções de serviço que filtram obrigatoriamente por `organizationId` ou pelo proprietário do registro para garantir multi-tenancy.
