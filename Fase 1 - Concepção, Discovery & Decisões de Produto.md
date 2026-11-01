# Fase 1: Concepção, Discovery & Decisões de Produto (ECC Universal)

Este documento detalha a primeira fase do ciclo de desenvolvimento de software da esteira de engenharia do **ECC (Enterprise Custom Configuration)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 1

Descobrir o problema real a ser resolvido, validar a hipótese de produto com foco no usuário primário, definir expressamente limites de escopo e resolver trade-offs ou decisões arquiteturais ambíguas antes de iniciar qualquer trabalho de especificação ou codificação.

---

## Trilhas e Possibilidades de Execução

Dependendo da maturidade da ideia e do nível de incerteza técnica ou de negócio, a Fase 1 disponibiliza três trilhas complementares:

### 1. Trilha Lean / PRD (`plan-prd`)
Indicada para features planejadas ou produtos novos que necessitam de um escopo claro e ágil:

* **Workflow**: [`plan-prd.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/plan-prd.md)
* **Atividades**:
  1. **Delimitação do Problema**: Identificar quem é o usuário primário e qual dor pontual está sendo resolvida.
  2. **Hipótese Mensurável**: Definir a métrica de sucesso (ex: redução de tempo de onboard em 40%, taxa de conversão > 15%).
  3. **Limites de Escopo (*Non-Goals*)**: Definir expressamente o que está fora de escopo para evitar inchaço precoce (*scope creep*).
  4. **Critérios de Lançamento**: Estabelecer requisitos mínimos para a entrega viável (*MVP*).

---

### 2. Trilha YC / Product Discovery (`product-lens`)
Indicada para pressão e teste de ideias em estágio inicial, validação de tração e ordenação de iniciativas:

* **Skill**: [`product-lens`](file:///home/willy/Sandbox/ecc/.agents/skills/product-lens/SKILL.md)
* **Atividades**:
  1. **Diagnóstico YC**: Questionamento rigoroso sobre a real necessidade do cliente, alternativas existentes e diferencial competitivo.
  2. **Auditoria de Time-to-Value (TTV)**: Medição do tempo necessário para que o usuário sinta o valor central da funcionalidade.
  3. **Priorização ICE**: Classificação de oportunidades por *Impact* (Impacto), *Confidence* (Confiança) e *Ease* (Facilidade de implementação).
  4. **Veredito Go / No-Go**: Avaliação objetiva recomendando avançar, pivotar ou descartar a ideia.

---

### 3. Decisões Complexas e Trade-offs (`council` / `council-multi-model`)
Indicada para decisões de alta consequência técnica ou de produto onde existem múltiplos caminhos válidos (ex: Monolito vs Microsserviços, SQL vs NoSQL, seleção de frameworks):

* **Skills**: [`council`](file:///home/willy/Sandbox/ecc/.agents/skills/council/SKILL.md) / [`council-multi-model`](file:///home/willy/Sandbox/ecc/.agents/skills/council-multi-model/SKILL.md)
* **Atividades**:
  1. **Conselho de 4 Vozes**: Convoca quatro perspectivas com papéis definidos para estruturar desacordo técnico saudável antes da síntese.
  2. **Crítica Multi-Model (Opcional)**: Submete a síntese a uma avaliação adversarial por um modelo ou provedor independente para testar fragilidades da proposta.
  3. **Síntese de Decisão**: Formaliza a recomendação consolidada justificando os trade-offs assumidos.

---

## Artefatos Gerados na Fase 1

* `.claude/prds/{feature}.prd.md`: Documento de Requisitos de Produto (PRD) com problema, métricas, escopo e requisitos de entrega.
* `PRODUCT-BRIEF.md`: Diagnóstico executivo gerado pela ferramenta `product-lens` com a recomendação Go/No-Go.
* Parecer de Conselho: Justificativa dos trade-offs técnicos adotados para alimentar a fase de especificações e ADRs.

---

## Próximo Passo: Transição para a Fase 2

Com o problema compreendido e o produto delineado, a esteira avança para a **Fase 2: Especificação Comportamental (SDD) & Design**:
* Definição de critérios estritos de aceitação: [`intent-driven-development`](file:///home/willy/Sandbox/ecc/.agents/skills/intent-driven-development/SKILL.md)
* Mineração de contratos comportamentais legados: [`spec-miner`](file:///home/willy/Sandbox/ecc/.agents/agents/spec-miner.md)
* Registro formal de decisões de arquitetura: [`architecture-decision-records`](file:///home/willy/Sandbox/ecc/.agents/skills/architecture-decision-records/SKILL.md)
* Modelagem e blindagem de tipos: [`type-design-analyzer`](file:///home/willy/Sandbox/ecc/.agents/agents/type-design-analyzer.md)
