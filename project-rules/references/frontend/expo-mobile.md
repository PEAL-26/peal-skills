# Frontend Mobile (Expo & React Native)

Padrões de desenvolvimento para aplicativos móveis (iOS e Android) com Expo SDK 55+ e React Native 0.83+.

> **Nota de versão:** o Expo emparelha cada SDK com uma versão exacta de React Native — SDK 55 traz RN 0.83 e React 19.2. A partir do SDK 55 a Arquitetura Legada deixou de existir: `newArchEnabled` já não é uma opção de `app.json` e qualquer biblioteca que ainda não suporte a Nova Arquitetura é um bloqueador.

---

## 1. Nomenclatura de Arquivos e Pastas

- **Kebab-Case Estrito em Tudo:** **Todos** os arquivos e diretórios do projeto devem utilizar `kebab-case` sem exceção.
  - Isso inclui classes TypeScript e componentes: `AudioAnalyzer` vive em `audio-analyzer.ts`, `UserProfile` vive em `user-profile.tsx`.
  - Proibido o uso de `PascalCase` ou `camelCase` em nomes de arquivos.

---

## 2. Proibição de Barrel Files em Código Compartilhado

- **Sem Re-exports Desnecessários:** Não crie arquivos barrel (`index.ts`) para reexportar tipos e funções utilitárias entre módulos diferentes.
- **Importação Direta da Origem:** Sempre importe diretamente do arquivo de origem (ex: `import { User } from '@/types/user'`). Isso previne dependências circulares e otimiza o tempo de resolução do Metro bundler.

---

## 3. Estrutura de Helpers e Utilitários

- **Helpers Reutilizáveis (Globais):** Funções compartilhadas por múltiplos módulos residem em `src/helpers/[nome].ts`.
- **Helpers Locais (Módulo):** Funções utilitárias exclusivas de um domínio residem em `helpers.ts` dentro da pasta daquele módulo (ex: `src/modules/audio/helpers.ts`). Não use nomes como `audio-helpers.ts`.
- **Funções Inline em Componentes:** Mantenha callbacks inline apenas se forem curtos (< 10 linhas). Se a função envolver múltiplos passos ou regras de negócio, extraia para um helper ou custom hook.

---

## 4. Sistema de Logging Mobile

- Utilize o logger centralizado `@/helpers/logger`. Nunca chame `console.log` diretamente.
- O logger mobile deve suportar filtragem por tags no painel de desenvolvedor interno:
  ```typescript
  logger.log("[AudioEngine] Decoding buffer started");
  logger.warn("[SyncWorker] Network request delayed, retrying...");
  logger.error("[Auth] Refresh token expired");
  ```
- Todas as tags novas devem ser registradas na constante `KNOWN_LOG_TAGS` do logger.
