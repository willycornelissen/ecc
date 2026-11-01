# Fase 8: Governança, Memória Contínua & Aprendizado (ECC Universal)

Este documento detalha a oitava e contínua fase da esteira de engenharia do **ECC (Enterprise Custom Configuration)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 8

Manter o contexto do projeto vivo e recuperável entre sessões de trabalho, aprender continuamente a partir das correções e preferências do desenvolvedor sem contaminação entre projetos, e combater o apodrecimento da documentação (*doc rot*), garantindo que arquitetura, mapas de código e regras reflitam a verdade do repositório em produção.

---

## Fluxo e Ferramentas de Execução

A Fase 8 opera de forma transversal e permanente através de três pilares:

### 1. Persistência de Memória de Sessão (`unified-memory` / `ck`)
Impede a perda de contexto quando sessões de chat são encerradas ou trocam de agente:

* **Skills**: [`unified-memory`](file:///home/willy/Sandbox/ecc/.agents/skills/unified-memory/SKILL.md) e [`ck`](file:///home/willy/Sandbox/ecc/.agents/skills/ck/SKILL.md) (Context Keeper)
* **Atividades**:
  1. **Salvar Estado da Sessão (`/save-session`)**: Persiste as decisões tomadas, arquivos tocados, tarefas concluídas e próximos passos recomendados no cofre de memória (*ECC Memory Vault*).
  2. **Retomar Trabalho (`/resume-session`)**: Injeta um resumo conciso do projeto na inicialização da próxima sessão, permitindo que o desenvolvedor continue exatamente de onde parou sem precisar re-explicar a arquitetura.
  3. **Interoperabilidade de Agentes**: Facilita a transferência de contexto entre diferentes modelos e harnesses (Claude Code, Cursor, Codex, OpenCode, Antigravity).

---

### 2. Aprendizado Contínuo com Sistema de Instintos (`continuous-learning-v2`)
Transforma correções pontuais do usuário em inteligência permanente para o projeto:

* **Skill**: [`continuous-learning-v2`](file:///home/willy/Sandbox/ecc/.agents/skills/continuous-learning-v2/SKILL.md)
* **Atividades**:
  1. **Observação via Hooks**: Detecta quando o usuário corrige um padrão de código, refatora uma abordagem sugerida pelo agente ou instrui uma preferência de estilo.
  2. **Criação de Instintos Atômicos**: Registra um "instinto" com gatilho (*trigger*), comportamento esperado (*action*) e pontuação de confiança (*confidence score*).
  3. **Escopo por Projeto**: Vincula os instintos ao projeto específico para evitar contaminação cruzada com outros repositórios.
  4. **Evolução de Regras**: Quando um instinto atinge alta frequência e confiança, é promovido formalmente a uma nova regra permanente em [`.agents/rules/`](file:///home/willy/Sandbox/ecc/.agents/rules) ou skill customizada.

---

### 3. Governança Ativa de Documentação e Codemaps (`living-docs-governance` / `doc-updater`)
Evita o descompasso crônico entre documentação e código (*drift*):

* **Skill & Subagente**: [`living-docs-governance`](file:///home/willy/Sandbox/ecc/.agents/skills/living-docs-governance/SKILL.md) e [`doc-updater`](file:///home/willy/Sandbox/ecc/.agents/agents/doc-updater.md)
* **Atividades**:
  1. **Atualização de Codemaps**: Gera e atualiza periodicamente os mapas de código em `docs/CODEMAPS/*`, permitindo que novos desenvolvedores e agentes naveguem rapidamente pelas camadas da aplicação.
  2. **Prevenção de Recriação Indesejada**: Mantém um registro de intenção de remoção (*deletion zone*) para evitar que agentes tentem recriar arquivos ou utilitários intencionalmente descontinuados.
  3. **Sincronização de Documentação**: Atualiza `README.md`, guias de arquitetura e documentação de API automaticamente após mudanças relevantes.

---

## Artefatos Gerados na Fase 8

* Cofre de memória persistente (`.agents/memory/` ou `.claude/memory/`).
* Banco de instintos do projeto (`instincts/`).
* Mapas de código atualizados (`docs/CODEMAPS/`).
* Regras refinadas e promovidas em [`.agents/rules/`](file:///home/willy/Sandbox/ecc/.agents/rules).

---

## Fechamento do Ciclo e Retroalimentação

A Fase 8 completa o ciclo contínuo de engenharia de software do ECC. Os aprendizados, instintos consolidados e a documentação viva alimentam diretamente o onboarding de novas funcionalidades na **Fase 0** e **Fase 1**, tornando o agente progressivamente mais inteligente, autônomo e alinhado aos padrões da equipe a cada nova iteração.
