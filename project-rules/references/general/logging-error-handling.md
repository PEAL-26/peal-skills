# Logging Centralizado e Tratamento de Erros

Regras para gerenciamento de logs, tipagem de erros e respostas estruturadas de API.

---

## 1. Padrão de Logging

- **Ponto Único de Entrada:** Todo log de aplicação deve ser realizado através do módulo centralizado `src/lib/logger.ts` (ou `@/helpers/logger`).
- **Níveis de Log:**
  - `logger.debug(message, meta?)` — Apenas em ambiente de desenvolvimento.
  - `logger.info(message, meta?)` — Operações normais informativas.
  - `logger.warn(message, meta?)` — Falhas esperadas ou com mecanismo fail-open.
  - `logger.error(source, meta?)` — Falhas inesperadas ou operacionais.
- **Integração Sentry / Monitoramento:** `logger.error` deve enviar a exceção para serviços de APM/Sentry de forma assíncrona e em bloco fail-open, garantindo que falhas de log nunca interrompam o fluxo principal do usuário.
- **Prefixos com Tags (Mobile / Node):** Utilize o padrão de tags entre colchetes para facilitar a filtragem:
  ```typescript
  logger.log("[AuthService] User session refreshed successfully");
  ```
- **Dados Sensíveis:** É estritamente proibido logar senhas, tokens de autenticação, chaves de API ou dados de cartão de crédito.

---

## 2. Padrão de Resposta de API (`ActionResult`)

Todas as mutações e chamadas de API internas ou Server Actions devem utilizar um contrato tipado e previsível:

```typescript
export type ActionResult<T> = {
  success: boolean;
  data: T | null;
  error: string | null;
  message?: string;
};

export type ListResult<T> = ActionResult<T[]> & {
  totalItems: number;
  totalPages: number;
  currentPage: number;
  prev: number | null;
  next: number | null;
};
```

### Helpers de Resposta:
```typescript
export function successResult<T>(data: T, message?: string): ActionResult<T> {
  return { success: true, data, error: null, message };
}

export function errorResult<T = null>(error: string): ActionResult<T> {
  return { success: false, data: null, error };
}
```

- **Consumo no Frontend:** O código cliente sempre deve verificar `if (!result.success)` para identificar erros, nunca verificar apenas a presença do campo `error`.
