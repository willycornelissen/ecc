# Fase 5: Qualidade, Segurança & Verificação Adversarial (ECC Universal)

Este documento detalha a quinta fase da esteira de engenharia do **ECC (Enterprise Custom Configuration)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 5

Garantir que nenhum código defeituoso, inseguro ou incompleto avance para produção, submetendo o código implementado na [Fase 4](file:///home/willy/Sandbox/ecc/Fase%204%20-%20Implementa%C3%A7%C3%A3o%20Guiada%20por%20Testes%20(TDD).md) a seis portões rigorosos de verificação mecânica, varreduras profundas de segurança e validação adversarial independente com barreira física de entrega.

---

## Portões de Validação e Ferramentas

A Fase 5 estrutura a garantia de qualidade através de cinco barreiras:

### 1. Portão dos 6 Critérios Mecânicos (`verification-loop`)
Executa uma bateria sequencial obrigatória sem tolerância a avisos suprimidos:

* **Skill**: [`verification-loop`](file:///home/willy/Sandbox/ecc/.agents/skills/verification-loop/SKILL.md)
* **Os 6 Portões**:
  1. **Build**: Compilação sem falhas em modo de produção.
  2. **Typecheck**: Verificação estrita de tipos sem supressões (`@ts-ignore`, `any` não justificado, casts inseguros).
  3. **Lint**: Conformidade estrita com padrões de formatação e linters do projeto.
  4. **Testes com Cobertura**: 100% dos testes unitários e de integração verdes com relatório de cobertura.
  5. **Grep de Segurança**: Varredura por segredos, chaves de API, credenciais hardcoded, comandos de debug (`console.log`, `debugger`, `print`).
  6. **Revisão de Diff**: Inspeção linha a linha das alterações para assegurar que apenas arquivos do plano foram tocados.

---

### 2. Auditoria de Segurança Especializada (`security-reviewer`)
Análise proativa com base nos padrões da indústria:

* **Subagente**: [`security-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/security-reviewer.md)
* **Atividades**:
  1. **OWASP Top 10**: Varre por injeção SQL/NoSQL, Cross-Site Scripting (XSS), Broken Object Level Authorization (BOLA), SSRF e desserialização insegura.
  2. **Tratamento de Dados Sensíveis**: Assegura sanitização de entradas, headers de segurança adequados e proteção de PII (*Personally Identifiable Information*).

---

### 3. Especialistas Complementares de Qualidade
* **Acessibilidade**: Subagente [`a11y-architect`](file:///home/willy/Sandbox/ecc/.agents/agents/a11y-architect.md) audita conformidade com WCAG 2.2 (navegação por teclado, contraste, leitores de tela).
* **Performance**: Subagente [`performance-optimizer`](file:///home/willy/Sandbox/ecc/.agents/agents/performance-optimizer.md) analisa vazamentos de memória, rerenderizações desnecessárias e custos de complexidade algorítmica.
* **Falhas Silenciosas**: Subagente [`silent-failure-hunter`](file:///home/willy/Sandbox/ecc/.agents/agents/silent-failure-hunter.md) busca erros suprimidos em blocos `try/catch` vazios e promessas rejeitadas sem tratamento.

---

### 4. Revisão Adversarial Dupla (`santa-method`)
Elimina o viés do agente autor através de avaliação cega e independente:

* **Skill**: [`santa-method`](file:///home/willy/Sandbox/ecc/.agents/skills/santa-method/SKILL.md)
* **Atividades**:
  1. **Dois Revisores Independentes**: O código é distribuído para dois agentes distintos munidos da mesma rubrica de avaliação.
  2. **Aprovação Convergente Obrigatória**: A funcionalidade só avança se **ambos** aprovarem a entrega de forma independente. Em caso de divergência, inicia-se um ciclo de correção e re-revisão.

---

### 5. Barreira Mecânica de Conclusão (`delivery-gate`)
Impede que o assistente declare uma tarefa como "pronta" antes do tempo:

* **Skill**: [`delivery-gate`](file:///home/willy/Sandbox/ecc/.agents/skills/delivery-gate/SKILL.md)
* **Atividades**: Intercepta comandos de finalização (*stop hooks*) e bloqueia a conclusão se houver falha em testes, evidência de racionalização superficial ou pendência de aprendizado.

---

## Artefatos Gerados na Fase 5

* Relatório de Verificação dos 6 Portões com status `PASS` ou `FAIL` documentado.
* Pareceres de segurança do `security-reviewer` e do `santa-method`.
* Relatórios de conformidade WCAG e análise de falhas silenciosas.

---

## Próximo Passo: Transição para a Fase 6

Com a aprovação em todos os portões de segurança e qualidade interna, a esteira avança para a **Fase 6: Testes Ponta a Ponta (E2E) e Auditoria de UI**:
* Automação de jornadas de usuário via Playwright: subagente [`e2e-runner`](file:///home/willy/Sandbox/ecc/.agents/agents/e2e-runner.md) e skill [`e2e-testing`](file:///home/willy/Sandbox/ecc/.agents/skills/e2e-testing/SKILL.md)
* Smoke tests pós-deploy e Core Web Vitals: [`browser-qa`](file:///home/willy/Sandbox/ecc/.agents/skills/browser-qa/SKILL.md)
* Auditoria de consistência em máquinas de estado na interface: [`click-path-audit`](file:///home/willy/Sandbox/ecc/.agents/skills/click-path-audit/SKILL.md)
* Medição estatística de confiabilidade de tarefas: [`eval-harness`](file:///home/willy/Sandbox/ecc/.agents/skills/eval-harness/SKILL.md)
