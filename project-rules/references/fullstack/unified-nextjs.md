# Fullstack Unificado (Next.js Monolítico)

Diretrizes para arquiteturas Fullstack em que Frontend e Backend coexistem na mesma base de código utilizando Next.js 16+ (App Router).

---

## 1. Fronteiras Rígidas entre Cliente e Servidor

Em um projeto fullstack unificado, o código de servidor e o código de navegador compartilham a mesma árvore de arquivos. É imperativo respeitar os limites:

1. **Server Actions (`'use server'`):**
   - Diretiva `'use server'` deve residir no topo do arquivo de ações (`actions.ts`).
   - Server Actions atuam como endpoints RPC: nunca confie em dados recebidos do chamador.
   - Todo parâmetro deve ser validado com Zod (`safeParse`).
   - Retorne sempre o tipo padrão `ActionResult<T> = { success, data, error }`.
2. **Proibição de Imports de Servidor no Cliente:**
   - Componentes que possuem a diretiva `'use client'` nunca devem importar arquivos de banco de dados (`prisma`, instâncias do Supabase com segredos, ou repositórios).
   - A comunicação do cliente para o servidor deve ocorrer exclusivamente via Server Actions ou chamadas HTTP para Route Handlers (`/api/...`).
3. **Proibição de Segredos no Cliente:**
   - Variáveis que começam com `NEXT_PUBLIC_` são embutidas no bundle JS no momento do build.
   - Segredos de banco, chaves privadas e `service_role` tokens só podem ser lidos em arquivos de servidor (Server Components, Route Handlers, Server Actions).

---

## 2. Padrão de Handlers e Server Actions

Para manter a testabilidade e legibilidade do código, separe a ação pública da lógica de negócio:

```text
src/server/customers/
├── handlers/
│   ├── create.ts     # Função assíncrona pura contendo a regra de negócio e SQL/ORM
│   ├── list.ts
│   └── update.ts
├── schema.ts         # Esquemas Zod para create e update
├── types.ts          # Tipos derivados
├── actions.ts        # 'use server' - valida com schema, pega sessão e chama o handler
└── index.ts          # Re-exporta ações e tipos para consumo seguro
```

### Exemplo de `actions.ts`:
```typescript
'use server';

import { createCustomerHandler } from './handlers/create';
import { createCustomerSchema } from './schema';
import { ActionResult } from '@/types/action';
import { getCurrentUserSession } from '@/lib/auth/session';

export async function createCustomerAction(input: unknown): Promise<ActionResult<any>> {
  // 1. Validar payload com Zod
  const parsed = createCustomerSchema.safeParse(input);
  if (!parsed.success) {
    return { success: false, data: null, error: 'Dados inválidos' };
  }

  // 2. Validar sessão e permissão no servidor
  const session = await getCurrentUserSession();
  if (!session) {
    return { success: false, data: null, error: 'Não autorizado' };
  }

  // 3. Executar o handler de domínio
  return createCustomerHandler(session.organizationId, parsed.data);
}
```

---

## 3. Cache e Ciclo de Vida do Next.js 16

- Ao habilitar `cacheComponents`, utilize a sintaxe moderna `use cache` junto com `cacheLife` e `cacheTag`.
- Nunca chame funções de contexto dinâmico como `cookies()`, `headers()` ou autenticação diretamente dentro de uma função `use cache` — resolva os dados no caller e passe apenas os parâmetros necessários para a função cacheada.
