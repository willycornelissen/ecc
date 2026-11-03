# Fase 2: Especificação Comportamental (SDD) & Design (ECC Universal)

Este documento detalha a segunda fase da esteira de engenharia do **ECC (Enterprise Custom Configuration)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 2

Transformar a visão de produto obtida na [Fase 1](file:///home/willy/Sandbox/ecc/Fase%201%20-%20Concep%C3%A7%C3%A3o,%20Discovery%20&%20Decis%C3%B5es%20de%20Produto.md) em contratos comportamentais precisos, observáveis e testáveis (SDD — *Specification-Driven Development*), registrar formalmente as decisões de arquitetura e projetar o sistema de tipos para impedir estados inválidos em tempo de compilação.

---

## Fluxo e Ferramentas de Execução

A Fase 2 combina quatro frentes essenciais de especificação:

### 1. Critérios de Aceitação Observáveis (`intent-driven-development`)
Garante que os requisitos de negócio e de engenharia sejam rigorosamente verificáveis por testes automatizados:

* **Skill**: [`intent-driven-development`](file:///home/willy/Sandbox/ecc/.agents/skills/intent-driven-development/SKILL.md)
* **Atividades**:
  1. **Critérios Atômicos (`AC-001`, `AC-002`)**: Estrutura cada requisito no formato estrito:
     * *Cenário*: Contexto e pré-condições.
     * *Ação*: A operação realizada pelo usuário ou sistema.
     * *Resultado Esperado*: Estado observável final.
     * *Invariantes (*Must Not*)*: Comportamentos estritamente proibidos (ex: nunca vazar tokens, nunca permitir saldo negativo).
     * *Método de Verificação*: Tipo de teste automatizado (unitário, integração ou E2E) que comprovará o requisito.
  2. **Desambiguação Prévia**: Elimina termos vagos como "rápido", "intuitivo" ou "seguro" por metas numéricas e mensuráveis.

---

### 2. Mineração de Especificações em Código Legado (`spec-miner`)
Indicada quando a funcionalidade interage com repositórios legados sem documentação prévia:

* **Subagente**: [`spec-miner`](file:///home/willy/Sandbox/ecc/.agents/agents/spec-miner.md)
* **Atividades**:
  1. **Análise de Código Existente**: Rastreia rotas, handlers, regras de negócio e testes atuais para extrair o comportamento tácito do sistema.
  2. **Padronização OpenSpec**: Gera blocos estruturados de requisitos e invariantes em `openspec/specs/<capability>/spec.md` com âncoras de teste mapeadas diretamente nos arquivos de código.

---

### 3. Registro de Decisões de Arquitetura (`architecture-decision-records`)
Documenta para a posteridade os motivos de escolhas estruturais relevantes:

* **Skill**: [`architecture-decision-records`](file:///home/willy/Sandbox/ecc/.agents/skills/architecture-decision-records/SKILL.md)
* **Atividades**:
  1. **Criação de ADR Numerada**: Cria arquivos padronizados em `docs/adr/NNNN-titulo.md`.
  2. **Registro de Trade-offs**: Detalha o contexto da decisão, alternativas avaliadas, consequências positivas e negativas e o impacto na manutenção futura do projeto.
  3. **Manutenção do Índice**: Atualiza `docs/adr/README.md` como catálogo consultável por humanos e agentes.

---

### 4. Blindagem do Design de Tipos (`type-design-analyzer`)
Garante robustez em nível de compilador antes de implementar as regras de negócio:

* **Subagente**: [`type-design-analyzer`](file:///home/willy/Sandbox/ecc/.agents/agents/type-design-analyzer.md)
* **Atividades**:
  1. **Tornar Estados Inválidos Irrepresentáveis**: Emprega uniões discriminadas, tipos opacos ou branded types para prevenir dados inconsistentes em tempo de execução.
  2. **Encapsulamento de Invariantes**: Assegura que propriedades essenciais dependam de construtores válidos e não de mutações arbitrárias.

---

## Artefatos Gerados na Fase 2

* `specs/{feature}.spec.md` (ou `openspec/specs/<capability>/spec.md`): Contrato comportamental contendo todos os critérios `AC-xxx` e invariantes.
* `docs/adr/NNNN-titulo.md`: Registro formal da decisão de arquitetura tomada.
* Interfaces, tipos e schemas de dados (ex: `types.ts`, `schemas.py`, structs de domínio).

---

## Próximo Passo: Transição para a Fase 3

Com os contratos comportamentais, as decisões de arquitetura e o design de tipos consolidados, a esteira avança para a **Fase 3: Planejamento Técnico de Implementação**:
* Quebra em passos atômicos de execução: subagente [`planner`](file:///home/willy/Sandbox/ecc/.agents/agents/planner.md)
* Elaboração de plano auto-contido: workflow [`prp-plan.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/prp-plan.md)
* Revisão visual e colaborativa do plano no navegador: [`plan-canvas`](file:///home/willy/Sandbox/ecc/.agents/skills/plan-canvas/SKILL.md)
