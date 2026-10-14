# Backend Prisma ORM & Better Auth

Regras para aplicações backend em Node.js utilizando Prisma ORM e Better Auth com PostgreSQL.

---

## 1. Padrões de Modelagem no Prisma (`schema.prisma`)

- **Nomes Claros:** Nomes de tabelas em `PascalCase` com mapeamento explícito `@@map("snake_case_table")`.
- **Anotações Obrigatórias:**
  - Chaves primárias com `@id @default(uuid())` ou `@default(cuid())`.
  - Timestamps padrão: `createdAt DateTime @default(now()) @map("created_at")` e `updatedAt DateTime @updatedAt @map("updated_at")`.
  - Índices explícitos em chaves estrangeiras e colunas de busca frequente: `@@index([organizationId])`.
- **Migrações:** Toda alteração de schema deve ser aplicada via `pnpm prisma migrate dev --name <descricao>`. Nunca edite tabelas de produção diretamente pelo banco sem migração correspondente.

---

## 2. Acesso a Dados — Regra de Ouro

- **Proibido Prisma no Controlador ou na UI:** Componentes, controladores HTTP e Server Actions **nunca** executam queries do Prisma diretamente.
- **Camada de Módulos / Serviços:** Todas as queries residem exclusivamente em serviços ou repositórios dedicados dentro de `src/modules/**/services/` ou `src/services/`.
- **Singleton Seguro do Prisma:** Mantenha a instância do `PrismaClient` centralizada em `src/lib/db.ts` utilizando a convenção global para evitar múltiplas conexões em desenvolvimento com hot reload:
  ```typescript
  import { PrismaClient } from '@prisma/client';

  const globalForPrisma = globalThis as unknown as { prisma: PrismaClient };

  export const prisma = globalForPrisma.prisma || new PrismaClient();

  if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
  ```

---

## 3. Autenticação com Better Auth

- **Separação Servidor vs Cliente:**
  - `auth.server.ts` — Inicialização do Better Auth com `prismaAdapter(prisma, { provider: 'postgresql' })`. **Nunca** importe este arquivo no cliente ou no bundle do browser.
  - `auth.client.ts` — Criação do cliente de browser (`createAuthClient()`).
- **Sessão Otimizada (`deferSessionRefresh`):**
  - Configure `session: { deferSessionRefresh: true }`. Isso torna leituras de sessão (`GET /api/auth/get-session`) puramente de consulta (read-only), sem escrita no banco a cada requisição.
- **Cookie Cache Desligado:** Nunca ative `session.cookieCache`. Sem ele, toda validação verifica a base de dados, garantindo que contas desativadas ou sessões revogadas percam o acesso imediatamente.
- **Plugin `nextCookies()`:** Deve ser obrigatoriamente o último plugin na lista de plugins do Better Auth para garantir a propagação correta dos cookies `Set-Cookie`.
- **Segredos:** `BETTER_AUTH_SECRET` e `BETTER_AUTH_URL` residem exclusivamente no `.env` e nunca devem ser expostos via `NEXT_PUBLIC_`.
