# Regras do Projeto (Frontend - Expo & React Native)

Este arquivo define as diretrizes para este aplicativo móvel desenvolvido com Expo e React Native.

## Perfil do Projeto
- **Tipo:** FRONTEND (Mobile iOS/Android)
- **Framework:** Expo SDK 54+ / React Native 0.81+
- **Linguagem:** TypeScript Estrito (`strict: true`)
- **Nomenclatura:** `kebab-case` estrito para todos os arquivos e pastas

---

## 📚 Referências Oficiais da Skill `project-rules`
- **Gerais:**
  - [Padrões de Código e Tipagem](../references/general/coding-standards.md)
  - [Os 5 Pilares de Segurança](../references/general/security.md)
  - [Git Workflow e Pré-Commit](../references/general/git-workflow.md)
  - [Formatação e Linting](../references/general/formatting-linting.md)
  - [Logging e Erros](../references/general/logging-error-handling.md)
- **Mobile Expo:**
  - [Frontend Expo Mobile](../references/frontend/expo-mobile.md)
  - [Formulários e Prevenção de Concorrência](../references/frontend/forms.md)

---

## 🛠️ Comandos Expo
- Iniciar: `npx expo start`
- Android: `npm run android`
- iOS: `npm run ios`
- Lint: `npm run lint`
- Typecheck: `npx tsc --noEmit`

---

## 🚫 O Que NUNCA Fazer
1. Nunca usar `PascalCase` ou `camelCase` em nomes de arquivos (use sempre `kebab-case`).
2. Nunca criar arquivos barrel (`index.ts`) para reexportar tipos e utils compartilhados (importe diretamente da origem para evitar dependências circulares no Metro).
3. Nunca colocar segredos ou chaves de serviço sob o prefixo `EXPO_PUBLIC_`.
4. Nunca chamar `console.log` solto no código móvel; use o logger taggeado `@/helpers/logger`.
