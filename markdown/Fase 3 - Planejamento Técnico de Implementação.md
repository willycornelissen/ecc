# Fase 3: Planejamento Técnico de Implementação (ECC Universal)

Este documento detalha a terceira fase da esteira de engenharia do **ECC (Everything Claude Code)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 3

Decompor a especificação comportamental gerada na [Fase 2](file:///home/willy/Sandbox/ecc/Fase%202%20-%20Especifica%C3%A7%C3%A3o%20Comportamental%20(SDD)%20&%20Design.md) em um plano técnico ordenado, com etapas atômicas, mapeamento explícito de arquivos a alterar/criar, estimativas de risco, dependências entre tarefas e validação visual prévia com o usuário antes de escrever qualquer código.

---

## Fluxo e Ferramentas de Execução

A Fase 3 organiza a estratégia de execução através de três componentes:

### 1. Quebra Técnica Estruturada (`planner`)
Subagente especialista na elaboração de planos de engenharia:

* **Subagente**: [`planner`](file:///home/willy/Sandbox/ecc/.agents/agents/planner.md)
* **Atividades**:
  1. **Análise de Dependências**: Identifica a ordem de construção ideal (ex: migrations de banco → modelos de domínio → contratos de API → UI/componentes).
  2. **Mapeamento de Arquivos Exatos**: Lista com precisão cirúrgica cada arquivo a ser criado ou modificado, prevenindo alterações desnecessárias.
  3. **Identificação de Riscos**: Aponta potenciais armadilhas de concorrência, quebra de retrocompatibilidade, limites de taxa ou gargalos de performance, definindo planos de mitigação.
  4. **Critérios de Conclusão por Etapa**: Especifica como cada passo individual será validado antes de avançar para o próximo.

---

### 2. Workflow de Execução Autônoma Contínua (`prp-plan`)
Projetado para planos que serão implementados por agentes autônomos sem interrupções constantes:

* **Workflow**: [`prp-plan.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/prp-plan.md)
* **Atividades**:
  1. **Plano Auto-Contido**: Agrupa contexto, trechos de código existentes, comandos de teste e instruções completas para que o executor não precise adivinhar intenções.
  2. **Gatilhos de Validação Intermediários**: Cada micro-etapa inclui o comando exato de teste ou verificação necessário para prosseguir.

---

### 3. Validação Visual e Interativa (`plan-canvas`)
Permite ao desenvolvedor inspecionar o plano de forma visual e iterar facilmente:

* **Skill**: [`plan-canvas`](file:///home/willy/Sandbox/ecc/.agents/skills/plan-canvas/SKILL.md)
* **Atividades**:
  1. **Renderização no Navegador**: Abre uma interface interativa local com o fluxo de trabalho detalhado.
  2. **Feedback Direto**: Permite apontar elementos, comentar sobre etapas, reordenar prioridades e aprovar ou solicitar ajustes no plano sem atrito.
  3. **Aprovação Formal**: Garante alinhamento completo entre humano e agente antes do início da implementação.

---

## Artefatos Gerados na Fase 3

* `plans/{feature}.plan.md`: Plano técnico de implementação atômico com dependências, arquivos mapeados, riscos e etapas de verificação.
* Anotações e aprovações registradas pelo `plan-canvas`.

---

## Próximo Passo: Transição para a Fase 4

Com o plano aprovado e os passos claramente delineados, a esteira avança para a **Fase 4: Implementação Guiada por Testes (TDD & Pair Programming)**:
* Condução estrita do ciclo Red-Green-Refactor: subagente [`tdd-guide`](file:///home/willy/Sandbox/ecc/.agents/agents/tdd-guide.md) e skill [`tdd-workflow`](file:///home/willy/Sandbox/ecc/.agents/skills/tdd-workflow/SKILL.md)
* Revisão especializada contínua por linguagem e stack
* Resolução cirúrgica de erros de compilação via resolvers de build
* Orquestração colaborativa com [`dev-team`](file:///home/willy/Sandbox/ecc/.agents/skills/dev-team/SKILL.md) ou execução controlada via [`loop-design-check`](file:///home/willy/Sandbox/ecc/.agents/skills/loop-design-check/SKILL.md)
