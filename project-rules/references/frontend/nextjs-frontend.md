# Frontend Next.js (App Router)

Diretrizes e melhores práticas para interfaces construídas com Next.js 16+, React 19, Tailwind CSS e shadcn/ui.

---

## 1. Princípios do App Router

- **Server Components por Padrão:** Layouts (`layout.tsx`) e Páginas (`page.tsx`) são React Server Components por omissão.
- **`'use client'` Restrito:**
  - Aplique `'use client'` apenas em componentes folha que demandam interatividade do usuário, hooks de ciclo de vida (`useState`, `useEffect`) ou APIs do navegador.
  - Layouts que necessitam de interatividade devem continuar sendo Server Components que importam um invólucro (wrapper) client-side.
- **Metadados:** Funções `metadata` ou `generateMetadata` só podem existir em Server Components. Exporte um ou outro, nunca ambos.
- **`proxy.ts` vs `middleware.ts`:** No Next.js 16, a proteção de rotas e o refresh de sessão passam a utilizar preferencialmente a convenção `proxy.ts`.

---

## 2. Gerenciamento de Estado de URL com `nuqs`

- Todo filtro de busca, paginação ou ordenação de tabelas deve ser persistido nos query parameters da URL usando `nuqs`.
- **Parâmetros Padrão:**
  - `?q=` — Busca textual (com debounce de 300ms).
  - `?page=` e `?perPage=` — Paginação numérica.
  - `?status=` — Filtro de enumeração (`null` remove o parâmetro da URL).
- **`<Suspense>` Obrigatório:** Páginas que consomem parâmetros de busca na URL via `nuqs` ou `useSearchParams` **devem** envolver o componente consumidor em um `<Suspense fallback={<TableSkeleton />}>` para permitir renderização streaming.

---

## 3. Data Fetching no Cliente (TanStack Query)

- Chamadas assíncronas do cliente para Route Handlers ou Services usam TanStack Query com modo estrito.
- **Invalidação por Prefixo:** Sempre que uma mutação for concluída com sucesso, invalide as queries associadas pelo prefixo da chave (ex: `queryClient.invalidateQueries({ queryKey: ['customers'] })`).
- **Estados Visuais de Primeira Classe:** Toda listagem ou tabela deve tratar 4 estados explicitamente:
  1. **Loading:** Skeleton animado (`<TableSkeleton />`).
  2. **Error:** Mensagem amigável com botão de ação "Tentar Novamente" (`<TableError />`).
  3. **Empty:** Ilustração/texto explicativo com botão de Call-To-Action para criar o primeiro registro (`<TableEmpty />`).
  4. **Data:** Tabela populada com dados.

---

## 4. Tabelas com Scroll Horizontal Responsivo

Para garantir que tabelas funcionem perfeitamente em telas pequenas e celulares sem quebrar o layout da página:

1. **Wrapper com Scroll:** O contêiner imediato da tabela deve ter `overflow-x-auto`.
2. **Largura Mínima na Tabela:** A tag `<table>` deve ter uma largura mínima explícita (ex: `min-w-[620px]`). Sem isso, a tabela comprime e os textos quebram.
3. **A Regra do `min-w-0` em Ancestrais:**
   - Em layouts Flex e Grid (como o `<main>` e os cards do dashboard), itens flex/grid têm `min-width: auto` por padrão no CSS, impedindo o encolhimento abaixo do conteúdo da tabela.
   - **Obrigatório:** Adicione a classe `min-w-0` em todos os elementos ancestrais flexíveis até a tabela (`<main className="flex-1 min-w-0">`).
