# Padrões de Código e Tipagem (Stack-Agnóstico)

Este documento estabelece as regras universais de escrita de código, tipagem e nomenclatura a serem aplicadas em qualquer projeto.

---

## 1. Idioma e Separação de Responsabilidades

- **Código e Identificadores:** **100% em Inglês**.
  - Nomes de variáveis, funções, métodos, classes, interfaces, tipos, enums, arquivos e diretórios devem ser escritos exclusivamente em inglês.
  - Tabelas e colunas de banco de dados, chaves de API, parâmetros de query e payloads HTTP devem ser em inglês (`snake_case` no banco e APIs, `camelCase` no código TypeScript).
  - Commits e mensagens de git devem ser em inglês.
- **Interface e Mensagens de Usuário:** **Em Português** (padrão `pt-AO` para Angola ou `pt-BR`/`pt-PT` conforme o projeto).
  - Títulos de página, rótulos de formulário, botões, mensagens de erro, alertas de toast, e-mails e placeholders devem ser em português.
  - **Regra de Ouro:** Nunca expor identificadores internos, códigos de erro crus ou valores de enum diretamente ao usuário. Use sempre mapeamentos explícitos (`code -> label`) com fallback neutro (ex: `'Desconhecido'`).

---

## 2. TypeScript Estrito

- **Configuração:** `strict: true` ativado em todos os `tsconfig.json`.
- **Proibição do `any`:** O tipo `any` é terminantemente proibido.
  - Em casos de dados externos ou dinâmicos não validados, utilize `unknown` e faça estreitamento de tipo (type narrowing) através de validação (Zod ou Type Guards).
  - Se uma exceção for estritamente necessária, adicione um comentário justificando o motivo técnico.
- **Tipos Explícitos para Objetos:**
  - Em funções ou variáveis que recebem objetos com **mais de 1 propriedade**, é **obrigatório declarar uma interface ou type nomeado**.
  - ❌ **Incorreto (inline):**
    ```typescript
    function processUser(user: { id: string; name: string; email: string }) { ... }
    ```
  - ✅ **Correto (tipo dedicado):**
    ```typescript
    export interface UserInput {
      id: string;
      name: string;
      email: string;
    }
    function processUser(user: UserInput) { ... }
    ```
- **Imutabilidade e Inferência:**
  - Prefira sempre `const` a `let`. Nunca utilize `var`.
  - Use `as const` para arrays e objetos de configuração estáticos.

---

## 3. Convenções de Nomenclatura

| Elemento | Convenção | Exemplo |
|---|---|---|
| **Arquivos e Pastas** | `kebab-case` | `user-profile.ts`, `data-table.tsx` |
| **Componentes React** | `PascalCase` | `UserProfile`, `DataTable` |
| **Interfaces e Types** | `PascalCase` | `CustomerRecord`, `ListResult<T>` |
| **Funções e Métodos** | `camelCase` | `getUserById()`, `calculateTotal()` |
| **Variáveis e Propriedades** | `camelCase` | `customerList`, `isActive` |
| **Custom Hooks** | `camelCase` (prefixo `use`) | `useCustomerList`, `useDebounce` |
| **Constantes Globais** | `SCREAMING_SNAKE_CASE` | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| **Enums** | `PascalCase` com valores em `UPPERCASE` | `UserRole.ADMIN`, `OrderStatus.PENDING` |
| **Colunas de Banco / DTOs de API** | `snake_case` | `organization_id`, `created_at` |

---

## 4. Tipografia e Textos de Interface (Copy)

- Evite travessões longos (`—`) em textos visíveis na interface:
  - Para títulos, rótulos curtos e breadcrumbs: utilize a bolinha `·` como separador (ex: `Redefinir Senha · App`).
  - Para frases corridas e explicações: utilize vírgula.
  - Exceção: tabelas com campos vazios podem utilizar `—` como placeholder de valor ausente.
- Valores monetários: formatar sempre com a moeda do projeto (ex: `1 234,56 Kz` para Angola). Nunca exibir apenas o código cru "AOA" solto.
