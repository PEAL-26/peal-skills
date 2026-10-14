# Backend NestJS (Arquitetura Modular)

Padrões de engenharia para aplicações de backend construídas com NestJS, TypeScript e arquitetura corporativa em camadas.

---

## 1. Arquitetura Modular e Estrutura de Pastas

Cada módulo de negócio é autocontido e desacoplado, respeitando a inversão de dependência:

```text
src/
├── common/             # Filtros globais, interceptors, guards, decorators
│   ├── filters/        # ExceptionFilter global formatando ActionResult
│   ├── guards/         # AuthGuard, RolesGuard, TenantGuard
│   └── interceptors/   # LoggingInterceptor, TransformInterceptor
├── config/             # Configurações validadas com Zod / Joi
└── modules/
    └── customers/
        ├── dto/        # Data Transfer Objects com validação (Zod ou class-validator)
        │   ├── create-customer.dto.ts
        │   └── update-customer.dto.ts
        ├── entities/   # Representação da entidade de domínio ou modelo de banco
        ├── customers.controller.ts  # Entrada HTTP (rotas, validação de DTO, status)
        ├── customers.service.ts     # Casos de uso e regras de negócio
        ├── customers.repository.ts  # Interface e abstração de persistência
        └── customers.module.ts      # Módulo NestJS registrando providers e exports
```

---

## 2. Injeção de Dependências e Desacoplamento

- **Controladores Finos:** O `Controller` deve apenas validar o DTO de entrada, extrair o tenant/usuário do `Request` e delegar a execução ao `Service`.
- **Proibição de Instanciação Direta:** Nunca utilize `new MyService()` ou `new MyRepository()`. Utilize sempre o sistema de injeção de dependências do NestJS via construtor.
- **Interfaces para Repositórios:** Defina interfaces para a camada de acesso a dados (`ICustomerRepository`) para facilitar testes unitários com mocks no Vitest/Jest.

---

## 3. Validação de DTOs e Sanitização de Entrada

- Configure o `ValidationPipe` global no `main.ts` com:
  ```typescript
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,              // Remove propriedades não declaradas no DTO
      forbidNonWhitelisted: true,    // Retorna erro se propriedades extras forem enviadas
      transform: true,              // Converte tipos primitivos automaticamente
    }),
  );
  ```

---

## 4. Filtro de Exceções Global (`ActionResult`)

Todas as exceções do NestJS (`HttpException`, erros de banco de dados, erros de validação) devem ser interceptadas por um `AllExceptionsFilter` para garantir o contrato padrão de resposta:

```typescript
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception instanceof HttpException ? exception.getStatus() : 500;
    const message = exception instanceof HttpException ? exception.message : 'Erro interno do servidor';

    response.status(status).json({
      success: false,
      data: null,
      error: message,
    });
  }
}
```
