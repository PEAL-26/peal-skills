# Os 5 Pilares de Segurança de Aplicações

Qualquer projeto — seja Fullstack, Backend ou Frontend — deve seguir rigorosamente estes cinco pilares fundamentais de segurança em todas as camadas da aplicação.

---

## 1. Banco Sem Tranca (Isolamento de Inquilino / Multi-Tenancy)

- **O Problema:** Dados de uma empresa ou usuário sendo visualizados ou alterados por outro devido à ausência de filtros automáticos de tenant ou owner.
- **Regras Obrigatórias:**
  - **No Supabase / PostgreSQL:** Todas as tabelas de negócio devem ter Row Level Security (RLS) ativado por omissão (`ALTER TABLE ... ENABLE ROW LEVEL SECURITY`). Políticas RLS são **deny-by-default**; nunca utilize `USING (true)` em tabelas autenticadas.
  - Multi-tenant obrigatório: Toda tabela de dados possui coluna `organization_id` ou `company_id`.
  - **Em APIs com Prisma / Drizzle / SQL Próprio:** Toda query de listagem, busca, agregação, relatório e mutação **deve** incluir explicitamente o filtro pelo tenant/usuário autenticado (`where: { organizationId, ... }`).
  - Nunca confie no tenant ID recebido pelo payload do cliente; derive o tenant ID exclusivamente da sessão validada no servidor.

---

## 2. Permissão Definida no Navegador (Server-Side Authorization)

- **O Problema:** O frontend oculta botões ou rotas baseado em papéis (`isAdmin`, `role === 'admin'`), mas o backend executa a operação sem validar os privilégios.
- **Regras Obrigatórias:**
  - O controle de acesso visual na interface é **apenas conveniência de UX**, nunca segurança.
  - **Toda** Server Action, rota de API (`Route Handlers`, endpoints REST) e mutação no backend deve verificar a sessão e os privilégios do usuário no servidor antes de executar a lógica de negócio.
  - Em Supabase: nunca use `raw_user_meta_data` para autorização (é editável pelo cliente); utilize `app_metadata` assinado via hooks ou tabela de membros com papéis (`organization_members.role`).

---

## 3. IDOR (Insecure Direct Object Reference)

- **O Problema:** Endpoints que buscam, alteram ou deletam registros por ID (`/api/items/:id`, Server Actions `deleteItem(id)`) sem verificar se o objeto pertence ao chamador.
- **Regras Obrigatórias:**
  - Sempre validar a relação de posse antes de qualquer leitura ou mutação.
  - O padrão de consulta deve ser composto: `WHERE id = :id AND organization_id = :currentOrgId`.
  - Em casos de recursos hierárquicos (ex: um comentário dentro de uma tarefa dentro de um projeto), valide a árvore de propriedade completa.

---

## 4. Chaves e Segredos Expostos (Zero Hardcoded Secrets)

- **O Problema:** Chaves secretas, senhas, `service_role` tokens ou chaves de webhook gravadas no código-fonte, commits git ou expostas no bundle do frontend.
- **Regras Obrigatórias:**
  - Segredos residem estritamente em variáveis de ambiente protegidas ou em Vault seguro (ex: Supabase Vault).
  - **Prefixo de Cliente:** Chaves que iniciam com `NEXT_PUBLIC_`, `EXPO_PUBLIC_` ou `VITE_` são **públicas** e visíveis no browser/dispositivo. **NUNCA** coloque uma chave de serviço, segredo de API ou chave privada sob esse prefixo.
  - A chave `service_role` do Supabase ou segredos de banco de dados **nunca** devem existir no cliente ou em builds desktop/mobile.
  - Validação no arranque: utilize esquemas Zod (ex: `src/configs/env.ts`) para validar a presença de variáveis obrigatórias no startup da aplicação, falhando imediatamente caso estejam ausentes.

---

## 5. Inputs Sem Tratamento (XSS & Injeção)

- **O Problema:** Renderização de conteúdo controlado pelo usuário sem escape ou sanitização, permitindo injeção de scripts (XSS).
- **Regras Obrigatórias:**
  - **No Frontend:**
    - Proibido o uso de `dangerouslySetInnerHTML`, `v-html` ou equivalentes sem uma biblioteca de sanitização auditada (ex: DOMPurify).
    - Links dinâmicos (`<a href={url}>`): Valide o protocolo de URLs fornecidas por usuários para rejeitar esquemas como `javascript:`.
    - Proibido o uso de `eval()` ou `new Function()`.
  - **No Backend:**
    - Validação de entrada na fronteira: todos os inputs de API e formulários devem passar por esquemas Zod estritos (`schema.safeParse()`).
    - Templates de e-mail e respostas HTML devem escapar caracteres especiais automaticamente.
