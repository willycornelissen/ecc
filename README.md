# ECC - Guia e Pipeline de Engenharia com Agentes

Documentação e esteira de engenharia baseadas no **ECC (Enterprise Custom Configuration / Everything Claude Code)**, adaptadas para o ecossistema multi-harness (Antigravity, Claude Code, Cursor, Codex, OpenCode).

---

## 📌 Visão Geral

Este repositório consolida o guia arquitetural, fluxos de trabalho e a especificação detalhada de todas as fases do ciclo de desenvolvimento de software (SDLC) orientado a agentes de IA autônomos e colaborativos.

Para uma introdução aos conceitos fundamentais e comparação com outras metodologias, consulte:
* [ecc.md](file:///home/willy/Sandbox/ecc/ecc.md) — O que é o ECC, arquitetura, quando utilizar e comparativo de mercado.
* [pipeline.md](file:///home/willy/Sandbox/ecc/pipeline.md) — Pipeline completa de desenvolvimento, visão geral em diagrama Mermaid e trilhas de execução.

---

## 🚀 Fases da Pipeline de Engenharia

A esteira de engenharia do ECC é composta por fases estruturadas, garantindo que o agente atue de forma disciplinada do problema à produção:

| Fase | Título | Foco Principal |
| :---: | :--- | :--- |
| **Fase 0** | [Onboarding & Setup](file:///home/willy/Sandbox/ecc/Fase%200%20-%20Onboarding%20&%20Setup.md) | Detecção de stack (`project-init`), mapeamento de código legado e calibração de contexto. |
| **Fase 1** | [Concepção, Discovery & Decisões de Produto](file:///home/willy/Sandbox/ecc/Fase%201%20-%20Concep%C3%A7%C3%A3o,%20Discovery%20&%20Decis%C3%B5es%20de%20Produto.md) | Validação de hipótese (`product-lens`), PRD Lean e resolução de trade-offs (`council`). |
| **Fase 2** | [Especificação Comportamental (SDD) & Design](file:///home/willy/Sandbox/ecc/Fase%202%20-%20Especifica%C3%A7%C3%A3o%20Comportamental%20(SDD)%20&%20Design.md) | Contratos comportamentais observáveis (`intent-driven-development`), ADRs e design de tipos. |
| **Fase 3** | [Planejamento Técnico de Implementação](file:///home/willy/Sandbox/ecc/Fase%203%20-%20Planejamento%20T%C3%A9cnico%20de%20Implementa%C3%A7%C3%A3o.md) | Quebra atômica de passos (`planner`), planos auto-contidos (`prp-plan`) e revisão visual (`plan-canvas`). |
| **Fase 4** | [Implementação Guiada por Testes (TDD)](file:///home/willy/Sandbox/ecc/Fase%204%20-%20Implementa%C3%A7%C3%A3o%20Guiada%20por%20Testes%20(TDD).md) | Ciclo estrito Red-Green-Refactor (`tdd-workflow`), revisores por stack e cura de build. |
| **Fase 5** | [Qualidade, Segurança & Verificação Adversarial](file:///home/willy/Sandbox/ecc/Fase%205%20-%20Qualidade,%20Seguran%C3%A7a%20&%20Verifica%C3%A7%C3%A3o%20Adversarial.md) | 6 portões mecânicos (`verification-loop`), OWASP Top 10 e revisão cruzada dupla (`santa-method`). |
| **Fase 6** | [Testes Ponta a Ponta (E2E) & Auditoria de UI](file:///home/willy/Sandbox/ecc/Fase%206%20-%20Testes%20Ponta%20a%20Ponta%20(E2E)%20&%20Auditoria%20de%20UI.md) | Automação Playwright (`e2e-runner`), auditoria de acessibilidade/Core Web Vitals (`browser-qa`). |
| **Fase 7** | [Publicação, Pull Request & Produção](file:///home/willy/Sandbox/ecc/Fase%207%20-%20Publica%C3%A7%C3%A3o,%20Pull%20Request%20&%20Produ%C3%A7%C3%A3o.md) | Auditoria de produção (`production-audit`), convenções limpas de Git e descrição de PR. |
| **Fase 8** | [Governança, Memória Contínua & Aprendizado](file:///home/willy/Sandbox/ecc/Fase%208%20-%20Governan%C3%A7a,%20Mem%C3%B3ria%20Cont%C3%ADnua%20&%20Aprendizado.md) | Memória de sessão (`unified-memory` / `ck`), instintos evolutivos e mapas de código (`CODEMAPS`). |

---

## 🛠️ Estrutura do Repositório

```text
.
├── README.md                                          # Visão geral e catálogo
├── pipeline.md                                        # Especificação completa da esteira SDLC
├── ecc.md                                             # Guia arquitetural do ECC e comparativos
├── Fase 0 - Onboarding & Setup.md                     # Fase 0: Inicialização e regras da stack
├── Fase 1 - Concepção, Discovery & Decisões de Produto.md
├── Fase 2 - Especificação Comportamental (SDD) & Design.md
├── Fase 3 - Planejamento Técnico de Implementação.md
├── Fase 4 - Implementação Guiada por Testes (TDD).md
├── Fase 5 - Qualidade, Segurança & Verificação Adversarial.md
├── Fase 6 - Testes Ponta a Ponta (E2E) & Auditoria de UI.md
├── Fase 7 - Publicação, Pull Request & Produção.md
├── Fase 8 - Governança, Memória Contínua & Aprendizado.md
└── .agents/                                           # Configuração e módulos do ECC instalado
    ├── agents/                                        # Subagentes especializados
    ├── rules/                                         # Regras operacionais e de linters por stack
    ├── skills/                                        # Playbooks e workflows sob demanda
    ├── workflows/                                     # Comandos e shims de orquestração
    └── ecc-install-state.json                         # Estado de instalação do ECC
```

---

## 📄 Licença

Este repositório é distribuído sob a licença [MIT](https://opensource.org/licenses/MIT).
