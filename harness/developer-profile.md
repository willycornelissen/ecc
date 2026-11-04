# Developer Profile: Recursos do Projeto

Este documento consolida os recursos disponíveis no projeto configurados via **ECC (Everything Claude Code / Universal Agent)** no diretório `.agents`.

## Visão Geral

| Tipo | Quantidade no Projeto | Localização | Descrição / Destaques |
| :--- | :---: | :--- | :--- |
| **Skills** | **125** | `.agents/skills/` | Guias e instruções especializadas (ex.: `api-design`, `tdd-workflow`, `golang-patterns`, `react-patterns`, `database-migrations`). *(+8 skills globais do ambiente Antigravity)* |
| **Agentes** | **68** | `.agents/agents/` | Especialistas delegáveis para revisão, compilação e arquitetura (ex.: `code-reviewer`, `security-reviewer`, `planner`, `build-error-resolver`). |
| **Rules** | **121** *(122 com o README)* | `.agents/rules/` | Diretrizes e padrões arquiteturais: 10 regras centrais/comuns (`common-*`) e 111 regras cobrindo mais de 20 linguagens e frameworks (Go, Python, TypeScript, Java, Rust, Angular, Vue, etc.). |
| **Workflows** | **94** | `.agents/workflows/` | Rotinas automatizadas de execução (ex.: `code-review`, `feature-dev`, `security-scan`, `plan-prd`, `quality-gate`, `checkpoint`). |

---

## Detalhamento por Categoria

### 1. Skills (125 no projeto)
Instruções estruturadas com padrões práticos de desenvolvimento, incluindo:
- **Backend & Linguagens**: `golang-patterns`, `python-patterns`, `rust-patterns`, `springboot-patterns`, `django-patterns`, `dotnet-patterns`, `laravel-patterns`, `nestjs-patterns`, `perl-patterns`, `kotlin-patterns`.
- **Frontend**: `react-patterns`, `vue-patterns`, `nuxt4-patterns`, `compose-multiplatform-patterns`, `frontend-design-direction`, `make-interfaces-feel-better`.
- **Bancos de Dados**: `postgres-patterns`, `mysql-patterns`, `redis-patterns`, `clickhouse-io`, `database-migrations`, `jpa-patterns`, `kotlin-exposed-patterns`.
- **Testes & Qualidade**: `tdd-workflow`, `golang-testing`, `python-testing`, `rust-testing`, `react-testing`, `e2e-testing`, `django-tdd`, `springboot-tdd`, `codehealth-mcp`.
- **Arquitetura & Engenharia**: `api-design`, `hexagonal-architecture`, `android-clean-architecture`, `error-handling`, `mcp-server-patterns`, `domain-modeling`.

### 2. Agentes (68 no projeto)
Subagentes especializados para tarefas autônomas e focadas:
- **Revisores de Código (Reviewers)**: `code-reviewer`, `security-reviewer`, `database-reviewer`, `performance-optimizer`, `go-reviewer`, `python-reviewer`, `react-reviewer`, `typescript-reviewer`, `rust-reviewer`, `java-reviewer`, `django-reviewer`, etc.
- **Resolução de Builds e Erros (Resolvers)**: `build-error-resolver`, `go-build-resolver`, `react-build-resolver`, `rust-build-resolver`, `java-build-resolver`, `python-build-resolver`, `swift-build-resolver`, etc.
- **Arquitetura e Planejamento**: `architect`, `code-architect`, `planner`, `gan-planner`, `network-architect`, `homelab-architect`.
- **Auditoria e Diagnóstico**: `silent-failure-hunter`, `comment-analyzer`, `conversation-analyzer`, `type-design-analyzer`, `pr-test-analyzer`, `docs-lookup`.

### 3. Rules (121 regras + 1 README)
Regras e convenções aplicadas para consistência e qualidade:
- **Comuns (10)**: `common-agents`, `common-code-review`, `common-coding-style`, `common-development-workflow`, `common-git-workflow`, `common-hooks`, `common-patterns`, `common-performance`, `common-security`, `common-testing`.
- **Específicas por Linguagem/Framework (111)**: Conjuntos padronizados (`coding-style`, `patterns`, `security`, `testing`, `hooks`) para:
  - Angular, ArkTS, C++, C#, Dart, F#, Golang, Java, Kotlin, Nuxt, Perl, PHP, Python, Quarkus, Rails, React, React Native, Rust, Spring Boot, Swift, TypeScript, Vue e Web em geral.

### 4. Workflows (94 no projeto)
Comandos executáveis e procedimentos operacionais padronizados:
- **Desenvolvimento & Ciclo de Vida**: `feature-dev`, `plan`, `plan-prd`, `quality-gate`, `checkpoint`, `auto-update`, `save-session`, `resume-session`.
- **Revisão & Testes**: `code-review`, `security-scan`, `test-coverage`, `pr`, `review-pr`, `prp-*`.
- **Compilação e Testes por Stack**: `go-build`, `go-test`, `react-build`, `react-test`, `rust-build`, `rust-test`, `gradle-build`, etc.
