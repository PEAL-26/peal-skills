# Git Workflow e Ciclo de Entrega

Este documento descreve o fluxo de versionamento, convenções de commits e verificações obrigatórias de qualidade antes de qualquer entrega ou push.

---

## 1. Estratégia de Branches

- **`main` / `master`:** Código de produção. Estável e sempre testado.
- **`development`:** Ramo de integração contínua (quando aplicável).
- **`feature/<nome>`:** Novas funcionalidades (ex: `feature/user-authentication`, `feature/payment-gateway`).
- **`fix/<nome>` ou `hotfix/<nome>`:** Correções de defeitos críticos (ex: `fix/invoice-total-rounding`).

---

## 2. Conventional Commits

Todas as mensagens de commit devem seguir a especificação [Conventional Commits](https://www.conventionalcommits.org/):

```text
<tipo>(<escopo-opcional>): <descrição curta em inglês>
```

### Tipos Permitidos
- **`feat`:** Uma nova funcionalidade para o usuário ou sistema.
- **`fix`:** Correção de um bug.
- **`refactor`:** Mudança de código que não corrige bug nem adiciona funcionalidade.
- **`perf`:** Melhoria de desempenho.
- **`style`:** Ajustes de formatação, espaços, sem impacto na lógica.
- **`test`:** Adição ou correção de testes automatizados.
- **`docs`:** Alteração em documentação (README, arquitetura, etc.).
- **`chore`:** Tarefas de build, dependências ou ferramentas de suporte.

---

## 3. Pipeline de Verificação Pré-Commit / Pré-Push

Antes de solicitar ou realizar um commit e push, execute sempre o ciclo completo de verificação local para garantir que a branch esteja verde:

```bash
# 1. Verificar lint e padrões de estilo
npm run lint    # ou pnpm lint

# 2. Verificar tipagem estrita
npx tsc --noEmit # ou pnpm typecheck

# 3. Formatar código automaticamente
npm run format  # ou pnpm format

# 4. Executar suíte de testes
npm test        # ou pnpm test

# 5. Executar build de produção (sem timeout)
npm run build   # ou pnpm build
```

### Regras Críticas:
1. **Nunca usar timeout no build:** Builds de Next.js, monorepos ou Vite com geração estática podem ser demorados em máquinas locais. Nunca interrompa por timeout.
2. **Corrigir todos os avisos e erros:** Qualquer erro ou aviso reportado por TypeScript, ESLint ou testes deve ser solucionado antes do commit. É proibido usar `// @ts-ignore` ou desativar regras de lint sem justificativa explícita.
3. **Migrações de Banco Imutáveis:** Migrações já aplicadas em produção ou integradas à branch principal **nunca** devem ser alteradas ou excluídas. Qualquer ajuste requer uma nova migração incremental.
