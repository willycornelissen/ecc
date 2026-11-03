# Fase 4: Implementação Guiada por Testes (TDD & Pair Programming) (ECC Universal)

Este documento detalha a quarta fase da esteira de engenharia do **ECC (Everything Claude Code)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 4

Executar o plano técnico elaborado na [Fase 3](file:///home/willy/Sandbox/ecc/Fase%203%20-%20Planejamento%20T%C3%A9cnico%20de%20Implementa%C3%A7%C3%A3o.md) adotando Desenvolvimento Guiado por Testes (TDD) estrito, com auxílio de especialistas por linguagem, auto-cura de falhas de compilação e orquestração controlada de agentes.

---

## Modos de Execução e Ferramentas

A Fase 4 opera através de quatro pilares de engenharia:

### 1. Ciclo Estrito Red-Green-Refactor (`tdd-guide` / `tdd-workflow`)
Garante que todo comportamento seja precedido por um teste com falha verificada:

* **Subagente & Skill**: [`tdd-guide`](file:///home/willy/Sandbox/ecc/.agents/agents/tdd-guide.md) e [`tdd-workflow`](file:///home/willy/Sandbox/ecc/.agents/skills/tdd-workflow/SKILL.md)
* **Atividades**:
  1. **Fase RED**: Escreve primeiro o teste automatizado baseado nos critérios `AC-xxx` da Fase 2 e executa-o para comprovar que ele falha pela razão correta.
  2. **Fase GREEN**: Implementa a quantidade mínima de código necessária para fazer o teste passar.
  3. **Fase REFACTOR**: Limpa o código, remove duplicações, melhora nomes e otimiza a legibilidade sem quebrar o teste.
  4. **Meta de Cobertura**: Exige cobertura mínima de 80%+ nas partes críticas e caminhos de negócio.

---

### 2. Especialistas Dedicados por Stack
O ECC ativa automaticamente revisores focados na linguagem e framework do projeto:

* **Revisores Automáticos**:
  * TypeScript / JavaScript: [`typescript-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/typescript-reviewer.md)
  * React / Next.js: [`react-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/react-reviewer.md)
  * Python / FastAPI / Django: [`python-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/python-reviewer.md) / [`fastapi-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/fastapi-reviewer.md) / [`django-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/django-reviewer.md)
  * Go: [`go-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/go-reviewer.md)
  * Rust: [`rust-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/rust-reviewer.md)
  * Java / Kotlin: [`java-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/java-reviewer.md) / [`kotlin-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/kotlin-reviewer.md)
  * Flutter / Dart: [`flutter-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/flutter-reviewer.md)
* **Atividades**: Aplicam convenções idiomáticas de linguagem, detectam vazamento de concorrência e asseguram boas práticas sem desviar da arquitetura.

---

### 3. Auto-Cura Cirúrgica de Compilação (*Build Resolvers*)
Quando ocorrem falhas de compilação, tipos ou empacotamento, especialistas em reparo entram em ação:

* **Resolvers**: [`build-error-resolver`](file:///home/willy/Sandbox/ecc/.agents/agents/build-error-resolver.md), [`react-build-resolver`](file:///home/willy/Sandbox/ecc/.agents/agents/react-build-resolver.md), [`rust-build-resolver`](file:///home/willy/Sandbox/ecc/.agents/agents/rust-build-resolver.md), [`go-build-resolver`](file:///home/willy/Sandbox/ecc/.agents/agents/go-build-resolver.md), [`cpp-build-resolver`](file:///home/willy/Sandbox/ecc/.agents/agents/cpp-build-resolver.md).
* **Atividades**: Analisam logs de erro do compilador e aplicam alterações mínimas e cirúrgicas para deixar o build verde rapidamente, sem refatorações amplas ou não autorizadas.

---

### 4. Orquestração e Modos de Pareamento

* **Equipe Simulada (`dev-team`)**:
  * Skill: [`dev-team`](file:///home/willy/Sandbox/ecc/.agents/skills/dev-team/SKILL.md)
  * Simula a colaboração de quatro papéis (*Product Manager*, *Arquiteto*, *Desenvolvedor* e *QA*) na mesma sessão para resolver desafios de implementação multidisciplinares.
* **Loops Autônomos Seguros (`loop-design-check` / `loop-operator`)**:
  * Skill: [`loop-design-check`](file:///home/willy/Sandbox/ecc/.agents/skills/loop-design-check/SKILL.md) e subagente `loop-operator`
  * Garante que loops autônomos de código possuam critérios de parada decidíveis por máquina (*machine-decidable goals*), prevenindo loops infinitos e alterações fora do plano.

---

## Artefatos Gerados na Fase 4

* Código-fonte de produção implementado e compilando sem erros.
* Suíte de testes unitários e de integração verdes e com cobertura >= 80%.

---

## Próximo Passo: Transição para a Fase 5

Com a implementação funcionalmente completa e testada em nível unitário/integração, a esteira avança para a **Fase 5: Qualidade, Segurança & Verificação Adversarial**:
* Execução dos 6 portões mecânicos de validação: [`verification-loop`](file:///home/willy/Sandbox/ecc/.agents/skills/verification-loop/SKILL.md)
* Varredura profunda de vulnerabilidades e credenciais: subagente [`security-reviewer`](file:///home/willy/Sandbox/ecc/.agents/agents/security-reviewer.md)
* Auditorias de acessibilidade WCAG 2.2 e performance
* Revisão adversarial dupla e independente: [`santa-method`](file:///home/willy/Sandbox/ecc/.agents/skills/santa-method/SKILL.md)
* Bloqueio mecânico de finalizações prematuras: [`delivery-gate`](file:///home/willy/Sandbox/ecc/.agents/skills/delivery-gate/SKILL.md)
