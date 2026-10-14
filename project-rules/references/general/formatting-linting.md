# Formatação e Linting

Configuração padrão de Prettier e ESLint para manter consistência absoluta em todos os projetos.

---

## 1. Configuração do Prettier

Todos os projetos utilizam Prettier com os plugins oficiais de ordenação de imports e classes Tailwind.

### Dependências
```bash
pnpm add -D prettier prettier-plugin-organize-imports prettier-plugin-tailwindcss
```

### Arquivo de Configuração (`prettier.config.js` ou `.prettierrc.js`)
```javascript
module.exports = {
  semi: true,
  singleQuote: true,
  trailingComma: 'all',
  printWidth: 100,
  tabWidth: 2,
  plugins: [
    'prettier-plugin-organize-imports',
    'prettier-plugin-tailwindcss'
  ],
};
```

### Scripts no `package.json`
```json
{
  "scripts": {
    "format": "prettier ./src --write",
    "format:check": "prettier ./src --check"
  }
}
```

---

## 2. Configuração do ESLint

- **Flat Config:** Utilizar ESLint v9+ com `eslint.config.mjs`.
- **Proibição de Bypass:** É proibido desativar regras nos arquivos de configuração do framework:
  - No Next.js: Nunca configure `typescript: { ignoreBuildErrors: true }` ou `eslint: { ignoreDuringBuilds: true }` no `next.config.ts`.
- **Regra do Console:**
  - `no-console: "error"` em arquivos de produção da aplicação.
  - O uso de `console.log` ou `console.error` solto no código é proibido. Todo log deve passar pelo logger centralizado.
  - Exceções: Arquivos de teste (`src/__tests__/**`), scripts CLI de manutenção e o próprio módulo `src/lib/logger.ts`.
