# Fase 6: Testes Ponta a Ponta (E2E) & Auditoria de UI (ECC Universal)

Este documento detalha a sexta fase da esteira de engenharia do **ECC (Everything Claude Code)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 6

Validar a experiência do usuário final nos navegadores e aplicações cliente, garantindo que as jornadas críticas funcionem de ponta a ponta sem erros de console, que as máquinas de estado de UI não entrem em condições de corrida, e que a interface atenda a padrões rígidos de acessibilidade, responsividade e estabilidade visual antes da publicação.

---

## Fluxo e Ferramentas de Execução

A Fase 6 compreende quatro frentes de validação de interface e usabilidade:

### 1. Automação de Jornadas E2E (`e2e-runner` / `e2e-testing`)
Automação de testes funcionais reais simulando o comportamento de navegação:

* **Subagente & Skill**: [`e2e-runner`](file:///home/willy/Sandbox/ecc/.agents/agents/e2e-runner.md) e [`e2e-testing`](file:///home/willy/Sandbox/ecc/.agents/skills/e2e-testing/SKILL.md)
* **Atividades**:
  1. **Page Object Model (POM)**: Estruturação dos fluxos em classes reutilizáveis com seletores robustos e acessíveis (`getByRole`, `getByLabel`), evitando dependência de classes CSS frágeis.
  2. **Jornadas Críticas de Negócio**: Automação de fluxos completos (ex: cadastro, login, checkout, preenchimento de formulários multipasso).
  3. **Quarentena de Flakiness**: Identificação e isolamento de testes intermitentes para garantir que a suíte em CI permaneça determinística.
  4. **Coleta de Evidências**: Geração automática de screenshots, vídeos e traces em caso de falha.

---

### 2. Smoke Tests de Navegador & Veredito de Release (`browser-qa`)
Verificação automatizada pré/pós-deploy em ambiente de homologação ou staging:

* **Skill**: [`browser-qa`](file:///home/willy/Sandbox/ecc/.agents/skills/browser-qa/SKILL.md)
* **Atividades**:
  1. **Erros de Console**: Captura e bloqueio imediato se houver qualquer erro não tratado (`TypeError`, `Uncaught Exception`, falhas 4xx/5xx de recursos).
  2. **Core Web Vitals**: Medição de métricas essenciais como LCP (*Largest Contentful Paint*), CLS (*Cumulative Layout Shift*) e FID/INP (*Interaction to Next Paint*).
  3. **Regressão Visual Multi-Breakpoint**: Captura comparativa de telas em Mobile (375px), Tablet (768px) e Desktop (1280px).
  4. **Auditoria Axe-Core**: Execução de varredura mecânica de acessibilidade resultando em um veredito binário formal: **SHIP** ou **DO-NOT-SHIP**.

---

### 3. Rastreamento de Máquinas de Estado na UI (`click-path-audit`)
Detecta bugs onde ações individuais funcionam isoladamente, mas produzem estados inconsistentes quando combinadas:

* **Skill**: [`click-path-audit`](file:///home/willy/Sandbox/ecc/.agents/skills/click-path-audit/SKILL.md)
* **Atividades**:
  1. **Mapeamento de Pontos de Toque**: Rastreia todos os botões, links e componentes interativos da interface.
  2. **Sequência de Transição de Estado**: Simula cliques repetidos rápidos (*double click*), transições concorrentes e cancelamentos no meio da operação para verificar se a interface permanece íntegra.

---

### 4. Harness de Confiabilidade e Regressão de Agente (`eval-harness`)
Mede a robustez do sistema e dos prompts frente a execuções repetidas:

* **Skill**: [`eval-harness`](file:///home/willy/Sandbox/ecc/.agents/skills/eval-harness/SKILL.md)
* **Atividades**:
  1. **Graders Baseados em Código e Regras**: Avaliação determinística da saída do sistema em cenários simulados.
  2. **Métricas de Confiabilidade**: Cálculo de `pass@k` (taxa de sucesso em k tentativas) e `pass^k` para garantir que o sistema não regrida entre versões de modelos ou refatorações de código.

---

## Artefatos Gerados na Fase 6

* Testes E2E implementados em `tests/e2e/` (Playwright / Puppeteer).
* Relatório consolidado do `browser-qa` com veredito **SHIP** / **DO-NOT-SHIP** e evidências visuais.
* Matriz de estados de botões e touchpoints gerada pelo `click-path-audit`.
* Relatório estatístico de confiabilidade `eval-harness`.

---

## Próximo Passo: Transição para a Fase 7

Com a experiência de ponta a ponta validada e a interface aprovada com veredito SHIP, a esteira avança para a **Fase 7: Publicação, Pull Request & Produção**:
* Auditoria local de prontidão para produção: [`production-audit`](file:///home/willy/Sandbox/ecc/.agents/skills/production-audit/SKILL.md)
* Abertura estruturada de Pull Request: workflow [`pr.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/pr.md)
* Higienização e empacotamento Open Source (se aplicável): subagentes `opensource-*`
