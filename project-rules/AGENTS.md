# Sistema Central de Regras de Engenharia (Project Rules)

Este repositório centraliza as regras, padrões de arquitetura e convenções técnicas extraídas dos projetos em produção do desenvolvedor.

## 🎯 Objetivo
Evitar redundâncias, divergências de estilo e falhas de segurança entre diferentes projetos, fornecendo uma base modular que pode ser consumida por projetos **Fullstack**, **Backend** ou **Frontend** em qualquer stack.

---

## 🏗️ Como Este Sistema Funciona

O sistema opera de forma hierárquica e sob demanda (Progressive Disclosure):

```text
project-rules/
├── SKILL.md                  # Orquestrador da Skill
├── AGENTS.md                 # Documento de entrada e índice central
├── references/               # Regras modulares (carregadas sob demanda)
│   ├── general/              # Regras universais (código EN, UI PT, 5 pilares, git, lint)
│   ├── frontend/             # Stacks e padrões de frontend (componentes, Next.js, Vite, Expo)
│   ├── backend/              # Stacks de backend (Supabase, NestJS, Prisma, Drizzle, Express)
│   └── fullstack/            # Padrões de unificação e separação (unificado, monorepo, polyrepo)
└── templates/                # Modelos de AGENTS.md prontos para copiar para a raiz dos projetos
```

---

## 🚀 Como Aplicar em um Projeto Novo ou Existente

1. **Escolha o Perfil do Projeto:**
   - **Fullstack Unificado:** Copie [`templates/FULLSTACK-UNIFIED-AGENTS.md`](./templates/FULLSTACK-UNIFIED-AGENTS.md) para a raiz do seu projeto como `AGENTS.md`.
   - **Fullstack Monorepo:** Copie [`templates/FULLSTACK-MONOREPO-AGENTS.md`](./templates/FULLSTACK-MONOREPO-AGENTS.md) para a raiz do monorepo e use os templates específicos de frontend e backend em cada workspace (`apps/web`, `packages/backend`, etc.).
   - **Fullstack Polyrepo:** Copie [`templates/FULLSTACK-POLYREPO-AGENTS.md`](./templates/FULLSTACK-POLYREPO-AGENTS.md).
   - **Backend Específico:** Utilize [`BACKEND-SUPABASE-AGENTS.md`](./templates/BACKEND-SUPABASE-AGENTS.md), [`BACKEND-NESTJS-AGENTS.md`](./templates/BACKEND-NESTJS-AGENTS.md) ou [`BACKEND-PRISMA-AGENTS.md`](./templates/BACKEND-PRISMA-AGENTS.md).
   - **Frontend Específico:** Utilize [`FRONTEND-NEXTJS-AGENTS.md`](./templates/FRONTEND-NEXTJS-AGENTS.md), [`FRONTEND-VITE-AGENTS.md`](./templates/FRONTEND-VITE-AGENTS.md) ou [`FRONTEND-EXPO-AGENTS.md`](./templates/FRONTEND-EXPO-AGENTS.md).

2. **Como o Agente Executa as Regras:**
   - Todo assistente de IA lê o `AGENTS.md` na raiz do projeto ao iniciar uma sessão.
   - O arquivo aponta diretamente para os módulos relevantes em `project-rules\references/`.
   - O agente carrega apenas as regras necessárias para a tarefa atual, sem sobrecarregar o orçamento de contexto.
