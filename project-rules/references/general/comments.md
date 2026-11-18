# Regras de Comentários no Código (Stack-Agnóstico)

Este documento estabelece as regras universais para escrita de comentários no código-fonte, aplicáveis a qualquer projeto (Fullstack, Backend ou Frontend), independentemente da stack tecnológica.

Objetivo: manter o código limpo, evitando comentários desnecessários que o próprio código já explica, e garantindo que os comentários que existam acrescentem informação que o código não consegue transmitir por si só.

---

## 1. Princípio Geral

- **O código é a fonte da verdade.** Nomes de funções, variáveis, classes e arquivos bem escolhidos substituem a maioria dos comentários.
- **Escreve comentário apenas quando o código não se explica por si só.** Se renomear uma variável ou extrair uma função elimina a necessidade do comentário, faz a refatoração em vez de escrever o comentário.
- **Não comentes o óbvio.** Exemplo: `// incrementa o contador em um` acima de `contador++` é desnecessário.

---

## 2. Comentário Agnóstico à Implementação

- **Comenta o "porquê", nunca o "como".** O código já diz como é feito; o comentário deve dizer por que foi feito dessa forma, qual a regra de negócio, decisão de arquitetura ou restrição externa que justifica a escolha.
- **Evite comentários que descrevam a implementação.** Se a implementação mudar, o comentário fica desatualizado e enganador.
- **Não referencie planos, tarefas ou funcionalidades específicas.** Código, comentários, nomes de ficheiros, variáveis, classes e funções nunca devem citar `plans/<id>`, nomes de funcionalidades em implementação ou identificadores de tarefas do plano. Planos mudam ao longo do tempo; um comentário preso a um plano desatualizado confunde quem mantém o código e os agentes de IA que tentam seguir um plano que já não é o correto.

---

## 3. Onde Comentários São Permitidos (e onde não são)

### ✅ Permitido

- **JSDoc / docstrings em APIs públicas:** funções, classes, interfaces e tipos exportados que são consumidos por outros módulos ou projetos podem ter comentário de interface (parâmetros, retorno, erros lançados), desde que não contenham referências a planos.
- **Decisões de arquitetura ou negócio não óbvias:** por exemplo, por que uma determinada validação existe, por que um endpoint usa um método HTTP específico, ou por que uma biblioteca foi escolhida sobre outra.
- **Avisos de segurança ou limitações conhecidas:** ex: "não alterar esta query sem validar o RLS da tabela".
- **Referências a issues ou bugs externos:** quando útil para rastrear contexto fora do repositório.

### ❌ Não Permitido

- **Comentários que apenas traduzem o código para palavras naturais:** `// soma a e b`, `// verifica se o usuário está ativo`.
- **Headers de arquivo com descrições do conteúdo:** não coloque blocos de comentário no topo do ficheiro a explicar o que o próprio ficheiro já explica (o nome do ficheiro e a sua estrutura já cumprem esse papel).
- **Referências a planos ou tarefas:** `// conforme definido no plans/20261014.1234-minha-funcionalidade`, `// tarefa X do plano Y`, `// implementação da funcionalidade Z do plano`.
- **Comentários de plano em nomes de código:** funções, variáveis, classes, arquivos ou pastas nunca devem incluir nomes de planos ou tarefas (ex: `implementarFuncionalidadePlanoX`, `planoX-service.ts`).

---

## 4. Quando Encontrar Comentários Inadequados no Código Existente

- **Não remova silenciosamente comentários de terceiros** sem perceber o contexto. Se um comentário parecer desnecessário, reporte ao Loop Development ou ao responsável pelo código antes de apagar.
- **Durante a implementação de uma tarefa**, se precisar tocar num ficheiro que contenha comentários desnecessários ou referências a planos, limpe apenas as partes relacionadas ao escopo da tarefa e registre a decisão no plano.
- **Na revisão final (Final Reviewer)**, comentários que violem estas regras são registados como pendências para correção.

---

## 5. Exemplos

### ❌ Incorreto

```typescript
// Soma dois números e retorna o resultado
function sum(a: number, b: number): number {
  return a + b;
}

// Implementação da funcionalidade de login conforme o plano 20261014.1234
export async function login(email: string, password: string) { ... }
```

### ✅ Correto

```typescript
// Usamos a soma aritmética direta porque os valores já foram
// validados como inteiros no middleware de entrada.
function sum(a: number, b: number): number {
  return a + b;
}

// Usamos o serviço externo de auth em vez do JWT local porque a
// política de segurança da empresa exige rotação de chaves a cada 30 minutos.
export async function login(email: string, password: string) { ... }
```
