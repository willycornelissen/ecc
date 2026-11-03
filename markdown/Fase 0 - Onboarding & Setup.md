# Fase 0: Onboarding & Setup (ECC Universal)

Este documento detalha o primeiro passo para a inicialização e o onboarding de um projeto utilizando a esteira de engenharia do **ECC (Enterprise Custom Configuration)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 0

Estabelecer as regras de desenvolvimento, mapear a stack tecnológica do projeto, compreender a arquitetura existente e calibrar o consumo da janela de contexto do agente.

---

## Fluxo de Execução

Dependendo do estado inicial do projeto, a Fase 0 divide-se em duas abordagens complementares:

### 1. Configurar o ECC para a Stack do Projeto (`project-init`)

Quando o objetivo é calibrar as regras, hooks e agentes especializados para a tecnologia específica do repositório:

* **Workflow**: [`project-init.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/project-init.md)
* **Atividades**:
  1. **Detecção da Stack**: Inspeciona manifestos do projeto (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, etc.).
  2. **Plano Dry-Run**: Determina o subconjunto ideal de regras em [`.agents/rules/`](file:///home/willy/Sandbox/ecc/.agents/rules) e skills, sem sobrescrever arquivos existentes.
  3. **Aprovação e Aplicação**: Apresenta ao desenvolvedor o plano detalhado de alterações antes de gravar ou modificar arquivos no repositório.

---

### 2. Mapear Repositório Existente (`codebase-onboarding`)

Quando o repositório já contém código-fonte legado ou em andamento e é necessário que o agente compreenda a arquitetura antes da codificação:

* **Skill**: [`codebase-onboarding`](file:///home/willy/Sandbox/ecc/.agents/skills/codebase-onboarding/SKILL.md)
* **Atividades**:
  1. **Reconhecimento (*Reconnaissance*)**: Mapeia a topologia de diretórios, frameworks adotados, convenções de testes e pontos de entrada (`main.*`, `index.*`, `src/`).
  2. **Mapeamento Arquitetural**: Documenta padrões de projeto, integrações de APIs, banco de dados e dependências estruturais.
  3. **Guia Inicial**: Gera documentação de referência rápida e mapa do projeto (`CLAUDE.md` / `CODEMAPS`).

---

### 3. Calibragem e Otimização de Contexto (`agent-sort` / `context-budget`)

Para evitar saturação prematura da janela de contexto e perda de foco do agente:

* **Separar Regras**: A skill [`agent-sort`](file:///home/willy/Sandbox/ecc/.agents/skills/agent-sort/SKILL.md) classifica regras e skills entre:
  * **DAILY**: Carregadas rotineiramente em cada interação (essenciais da stack).
  * **LIBRARY**: Carregadas apenas sob demanda para tarefas específicas.
* **Auditar Consumo**: A skill [`context-budget`](file:///home/willy/Sandbox/ecc/.agents/skills/context-budget/SKILL.md) e [`config-gc`](file:///home/willy/Sandbox/ecc/.agents/skills/config-gc/SKILL.md) inspecionam o uso de tokens e eliminam componentes redundantes.

---

## Artefatos Gerados na Fase 0

* [`ecc-install-state.json`](file:///home/willy/Sandbox/ecc/.agents/ecc-install-state.json): Registro de módulos, skills e regras ativas para o ambiente.
* [`.agents/rules/`](file:///home/willy/Sandbox/ecc/.agents/rules): Regras de codificação, segurança, testes e padrões calibradas para a stack.
* `CLAUDE.md` / `docs/CODEMAPS`: Documentação viva e mapa de arquitetura do repositório.

---

## Próximo Passo: Transição para a Fase 1

Com o ambiente e a arquitetura devidamente mapeados na Fase 0, a esteira avança para a **Fase 1: Concepção, Discovery e Decisões de Produto**:
* Workflow de PRD Lean: [`plan-prd.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/plan-prd.md)
* Diagnóstico de Produto & Tração: [`product-lens`](file:///home/willy/Sandbox/ecc/.agents/skills/product-lens/SKILL.md)
* Resolução de Trade-offs e Decisões Ambíguas: [`council`](file:///home/willy/Sandbox/ecc/.agents/skills/council/SKILL.md)
