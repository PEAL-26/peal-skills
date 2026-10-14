# Backend Express & Fastify (APIs REST Minimalistas)

Diretrizes para serviços HTTP rápidos e escaláveis utilizando Express ou Fastify com TypeScript estrito.

---

## 1. Arquitetura em Camadas (Layered Pattern)

Organize a aplicação dividindo as responsabilidades de forma clara e linear:

```text
src/
├── controllers/      # Extração de parâmetros HTTP e devolução de resposta JSON
├── services/         # Casos de uso e regras de negócio puras
├── repositories/     # Consultas ao banco de dados (SQL puro ou Query Builder)
├── middlewares/      # Autenticação, rate limit, logging, tratamento de erros
├── validations/      # Schemas Zod de validação de request (body, query, params)
├── routes/           # Mapeamento de rotas HTTP para controladores
└── app.ts            # Configuração de middlewares e inicialização do servidor
```

---

## 2. Validação Obrigatória de Entrada

Nenhuma requisição deve chegar ao controlador sem antes ser validada por um middleware Zod:

```typescript
import { Request, Response, NextFunction } from 'express';
import { AnyZodObject } from 'zod';

export const validateRequest = (schema: AnyZodObject) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      req.body = await schema.parseAsync(req.body);
      next();
    } catch (error) {
      return res.status(400).json({
        success: false,
        data: null,
        error: 'Dados de entrada inválidos',
        details: error,
      });
    }
  };
};
```

---

## 3. Middleware de Erro Global

Todas as rotas devem capturar exceções não tratadas através de um middleware global posicionado no final da cadeia de handlers:

```typescript
export function errorHandler(err: any, req: Request, res: Response, next: NextFunction) {
  const statusCode = err.statusCode || 500;
  const message = err.isOperational ? err.message : 'Erro interno do servidor';

  // Log centralizado
  logger.error(`[API Error] ${req.method} ${req.url}: ${err.message}`, { stack: err.stack });

  res.status(statusCode).json({
    success: false,
    data: null,
    error: message,
  });
}
```
