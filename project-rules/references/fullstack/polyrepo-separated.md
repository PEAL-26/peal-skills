# Fullstack Separado (Polyrepo)

Diretrizes para arquiteturas onde o Frontend e o Backend residem em repositórios ou pastas completamente independentes comunicando-se exclusivamente via rede (HTTP/REST ou WebSocket).

---

## 1. Sincronização de Contratos de API

Quando o frontend e o backend residem em repositórios separados, a maior causa de falhas é o desalinhamento de contratos (mudança de nomes de campos, tipos ou rotas).

### Estratégias Obrigatórias:
1. **Esquema como Fonte da Verdade:**
   - Utilize OpenAPI / Swagger ou gere DTOs TypeScript a partir da especificação da API.
   - Sempre que o backend alterar um payload de resposta ou adicionar uma rota, a documentação ou pacote de tipos de API deve ser versionado antes de ser consumido pelo frontend.
2. **Contrato de Resposta Padronizado:**
   - O backend sempre retorna no padrão:
     ```json
     {
       "success": true,
       "data": { ... },
       "error": null
     }
     ```
   - O frontend nunca deve assumir que os dados vêm desempacotados sem verificar `success === true`.

---

## 2. Autenticação e CORS

- **CORS Seguro:**
  - O backend nunca deve permitir `Access-Control-Allow-Origin: *` quando houver envio de cookies ou credenciais (`credentials: 'include'`).
  - Configure origens permitidas explicitamente a partir de variáveis de ambiente do backend.
- **Tokens e Sessões:**
  - Prefira tokens de sessão HTTP-Only via cookies ou Bearer Tokens armazenados de forma segura (SecureStore no mobile).
  - Trate a expiração do token (HTTP 401) com interceptors de rede no frontend para tentar refresh automático antes de deslogar o usuário.

---

## 3. Gestão de Ambientes

- O frontend deve configurar sua URL de API através de variável de ambiente (`VITE_API_URL` ou `NEXT_PUBLIC_API_URL`).
- Nunca utilize `http://localhost:3000` hardcoded no código do cliente.
