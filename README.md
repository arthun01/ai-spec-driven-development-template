# AI-SDD Template — React + Vite

> Template de desenvolvimento orientado por especificações com IA para projetos **React 18+ · TypeScript · Vite · Tailwind CSS · Zustand · React Query**.

---

## O que é AI-SDD?

**AI-Specification-Driven Development** é uma metodologia onde a IA conduz o ciclo completo de desenvolvimento — da visão do produto até o code review — com base em documentos de especificação estruturados e versionados.

Em vez de pedir para a IA "criar uma feature", você segue um fluxo controlado:

```
Vision → Product Map → Roadmap → PRD → Design → Tech Spec → Tasks → Implementação → Review
```

Cada etapa gera um documento que alimenta a próxima, garantindo rastreabilidade, consistência e qualidade.

---

## Estrutura do Template

```
.
├── AGENTS.md                        # Diretrizes de desenvolvimento (adapte ao seu projeto)
├── README.md                        # Este arquivo
└── .opencode/
    ├── commands/                     # Slash commands para a fase de planejamento
    │   ├── cria-vision.md           # /cria-vision — cria documento de visão
    │   ├── cria-product-map.md      # /cria-product-map — mapeia fluxos de usuário
    │   ├── cria-roadmap.md          # /cria-roadmap — organiza fases de implementação
    │   ├── cria-prd.md              # /cria-prd — cria PRD de funcionalidade
    │   ├── cria-design.md           # /cria-design — cria/atualiza protótipo visual
    │   ├── cria-techspec.md         # /cria-techspec — cria especificação técnica
    │   └── cria-tasks.md            # /cria-tasks — decompõe em tarefas
    ├── agents/                       # Subagents para a fase de execução
    │   ├── dev.md                   # Implementa tarefas (aciona executa-task)
    │   └── reviewer.md              # Revisa PRs (aciona executa-review)
    └── skills/                       # Skills com procedimentos detalhados
        ├── cria-vision/             # Documento de visão do produto
        ├── cria-product-map/        # Mapa de fluxos de usuário
        ├── cria-roadmap/            # Roadmap de fases
        ├── cria-prd/                # PRD (Product Requirements Document)
        │   └── assets/
        │       └── prd-template.md
        ├── cria-design/             # Orquestrador de design
        │   └── assets/
        │       └── design-template.md
        ├── design-in-paper/         # Modo Paper (delegada por cria-design)
        ├── paper-to-react/          # Converte artboards Paper em React/Tailwind/TS
        ├── mcp-paper/               # Referência das tools do Paper MCP
        ├── cria-techspec/           # Especificação técnica
        │   └── assets/
        │       └── techspec-template.md
        ├── cria-tasks/              # Decomposição em tarefas
        │   └── assets/
        │       ├── tasks-template.md
        │       └── task-template.md
        ├── executa-task/            # Implementação de tarefas
        └── executa-review/          # Code review
            ├── assets/
            │   └── review-report-template.md
            └── references/
                └── code-quality-checklist.md
```

Ao executar o fluxo, a seguinte estrutura é gerada no seu projeto:

```
ai-sdd/
├── system/                          # Documentos de sistema (gerados uma vez)
│   ├── vision.md                    # Visão do produto
│   ├── product_map.md               # Mapa de fluxos
│   └── roadmap.md                   # Roadmap de fases
└── prd-[feature-slug]/              # Um diretório por funcionalidade
    ├── prd.md                       # Requisitos de produto
    ├── design.md                    # Protótipo visual da feature
    ├── techspec.md                  # Especificação técnica
    ├── tasks.md                     # Resumo de tarefas
    └── tasks/                       # Tarefas individuais
        ├── 1_task.md
        ├── 2_task.md
        └── ...
```

---

## Como Usar

### 1. Copie para o seu projeto React

Copie a pasta `.opencode/` e o `AGENTS.md` para a raiz do seu projeto React:

```bash
cp -r .opencode/ /caminho/do/seu/projeto/
cp AGENTS.md /caminho/do/seu/projeto/
```

### 2. Adapte o AGENTS.md

O `AGENTS.md` vem pré-configurado para a stack **React 18+ · TypeScript · Vite · Tailwind CSS · Zustand · React Query**. Adapte os seguintes pontos ao seu projeto:

- **Linha 1-3**: Nome e descrição do projeto
- **Seção 2**: Definições exatas do frontend
- **Seção 4**: Comandos customizados (se aplicável)

### 3. Execute o fluxo AI-SDD

A fase de **planejamento** usa slash commands. A fase de **execução** usa subagents dedicados.

| Etapa | Gatilho | O que faz |
|-------|---------|-----------|
| 1 | `/cria-vision` | Define problema, público e proposta de valor |
| 2 | `/cria-product-map` | Mapeia fluxos de usuário por persona |
| 3 | `/cria-roadmap` | Organiza implementação em fases |
| 4 | `/cria-prd` | Cria PRD para uma funcionalidade |
| 5 | `/cria-design` | Cria/atualiza protótipo visual da feature |
| 6 | `/cria-techspec` | Traduz PRD em decisões arquiteturais |
| 7 | `/cria-tasks` | Decompõe em tarefas incrementais |
| 8 | agent `dev` | Implementa uma tarefa com testes |
| 9 | agent `reviewer` | Code review do PR no GitHub |

> **Nota**: Cada gatilho ativa uma skill que guia a IA por um processo estruturado com perguntas de esclarecimento, alinhamento com o usuário e checklists de qualidade.

---

## Fluxo Detalhado

### Fase de Planejamento (uma vez por projeto)

```mermaid
graph LR
    A[Vision] --> B[Product Map]
    B --> C[Roadmap]
```

- **Vision** — Define o problema, público-alvo e proposta de valor. Documento estável que não muda frequentemente.
- **Product Map** — Mapeia os fluxos de usuário organizados por persona. Descreve comportamento, não implementação.
- **Roadmap** — Organiza as funcionalidades em fases incrementais com dependências e prioridades.

### Fase de Especificação (uma vez por funcionalidade)

```mermaid
graph LR
    D[PRD] --> DS[Design]
    DS --> E[Tech Spec]
    E --> F[Tasks]
```

- **PRD** — Define O QUE e PORQUÊ de uma funcionalidade. Requisitos funcionais numerados (RF-XXX) para rastreabilidade.
- **Design** — Materializa visualmente as telas a partir do PRD.
- **Tech Spec** — Define COMO implementar. Árvore de componentes, gestão de estado, e endpoints de API. Consome o design aprovado como referência visual.
- **Tasks** — Decompõe em tarefas incrementais. Cada tarefa é um entregável funcional com testes.

### Fase de Execução (uma vez por tarefa)

```mermaid
graph LR
    G[Implementação] --> H[Review]
```

- **Implementação** — Subagent `dev` executa a tarefa (skill `executa-task`) seguindo PRD + Design + Tech Spec + AGENTS.md. Inclui testes e linting de JS/TS.
- **Review** — Subagent `reviewer` (skill `executa-review`) revisa o PR via GitHub MCP com comentários inline.

---

## Stack Suportada

Este template é pré-configurado para:

| Tecnologia | Versão | Uso |
|-----------|--------|-----|
| React | 18+ | Framework UI |
| TypeScript | 5+ | Linguagem |
| Vite | — | Bundler / Dev Server |
| Tailwind CSS | 3+ | Estilização |
| Zustand | — | Gestão de Estado Global |
| React Query | 5+ | Data Fetching / Caching |
| React Router | 6+ | Roteamento |
| Vitest / RTL | — | Testes Unitários e Componentes |
| ESLint | — | Linting |

> Para usar com outra stack (ex: Next.js), adapte o `AGENTS.md`, os templates em `assets/` e as referências nos skills de execução (`executa-task`, `executa-review`).

---

## Compatibilidade com Ferramentas de IA

Este template suporta nativamente várias ferramentas de desenvolvimento assistido por IA, mantendo os arquivos `.opencode/` como **Single Source of Truth** e utilizando symlinks para garantir compatibilidade perfeita e sincronia das regras:

- **Antigravity** — Use a pasta `.antigravity/` já configurada no template.
- **Cursor** — Copie os skills para `.cursor/rules/`
- **Windsurf** — Copie para `.windsurfrules/`
- **Cline / Roo Code** — Use como instruções customizadas

O conteúdo dos skills é agnóstico à ferramenta — são procedimentos sequenciais em markdown que qualquer LLM pode seguir.

---

## Licença

MIT
