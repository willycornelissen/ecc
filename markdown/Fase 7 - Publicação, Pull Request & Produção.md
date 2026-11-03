# Fase 7: Publicação, Pull Request & Produção (ECC Universal)

Este documento detalha a sétima fase da esteira de engenharia do **ECC (Everything Claude Code)**, conforme estruturado no [`pipeline.md`](file:///home/willy/Sandbox/ecc/pipeline.md).

---

## Objetivo da Fase 7

Auditar a prontidão do software para o mundo real ("o que pode quebrar em produção?"), padronizar o histórico do repositório Git com commits atômicos, formatar e abrir Pull Requests com descrições estruturadas e evidências de teste, além de preparar e higienizar o repositório caso o projeto seja publicado como código aberto.

---

## Fluxo e Ferramentas de Execução

A Fase 7 compreende três frentes operacionais de lançamento e integração:

### 1. Auditoria de Prontidão para Produção (`production-audit`)
Responde de forma preventiva à pergunta: *O que pode quebrar em produção?*

* **Skill**: [`production-audit`](file:///home/willy/Sandbox/ecc/.agents/skills/production-audit/SKILL.md)
* **Atividades**:
  1. **Variáveis de Ambiente e Configurações**: Verifica se novas variáveis foram documentadas em `.env.example`, se há validação de tipagem na inicialização e se existem segredos vazados.
  2. **Estratégia de Rollback**: Avalia se as migrations de banco de dados são retrocompatíveis (*expand-and-contract pattern*) e se a aplicação tolera reversão imediata de versão sem corromper dados.
  3. **Observabilidade e Logs**: Confirma que erros de negócio contêm logs estruturados com contexto suficiente (sem despejar dados sensíveis ou PII) e que métricas vitais estão monitoradas.
  4. **Timeouts e Resiliência**: Valida a presença de limites de timeout e retry em todas as chamadas remotas a microsserviços e APIs externas.

---

### 2. Abertura e Revisão de Pull Requests (`pr` / `common-git-workflow`)
Padroniza a entrega no Git com rastreabilidade completa:

* **Workflow & Regra**: [`pr.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/pr.md) / [`review-pr.md`](file:///home/willy/Sandbox/ecc/.agents/workflows/review-pr.md) e [`common-git-workflow.md`](file:///home/willy/Sandbox/ecc/.agents/rules/common-git-workflow.md)
* **Atividades**:
  1. **Organização da Branch**: Assegura branch temática no formato convencional (`feature/...`, `fix/...`, `refactor/...`) e histórico limpo sem commits temporários do tipo "wip" ou "fix tests".
  2. **Descrição Estruturada do PR**:
     * *Resumo das Mudanças*: O que mudou e por quê.
     * *Critérios de Aceitação Atendidos*: Vínculo com os critérios `AC-xxx` definidos na Fase 2.
     * *Evidências de Teste*: Comandos executados e resultados de testes unitários, integração e E2E.
     * *Instruções de Teste Manual*: Passos explícitos para o revisor humano reproduzir o fluxo.
  3. **Checklist Pré-Merge**: Validação de build em CI, revisão de pares e ausência de conflitos com a branch principal.

---

### 3. Pipeline de Sanitização e Publicação Open Source (Opcional)
Indicada quando o projeto ou biblioteca será liberado publicamente:

* **Subagentes**:
  * [`opensource-forker`](file:///home/willy/Sandbox/ecc/.agents/agents/opensource-forker.md): Cria fork seguro, remove credenciais, substitui referências corporativas internas por placeholders genéricos e limpa o histórico de commits.
  * [`opensource-sanitizer`](file:///home/willy/Sandbox/ecc/.agents/agents/opensource-sanitizer.md): Executa varredura profunda com mais de 20 padrões regex para certificar que nenhum segredo, chave de API ou PII residual permaneça no código, gerando relatório formal com status `PASS`.
  * [`opensource-packager`](file:///home/willy/Sandbox/ecc/.agents/agents/opensource-packager.md): Gera o pacote completo para distribuição aberta (`README.md`, `LICENSE`, `CONTRIBUTING.md`, templates de issues e `setup.sh`).

---

## Artefatos Gerados na Fase 7

* Relatório de Prontidão de Produção (`production-readiness.md`).
* Pull Request formatado e submetido na plataforma de versionamento (GitHub/GitLab).
* Pacote de publicação aberto devidamente higienizado (caso aplicável).

---

## Próximo Passo: Transição para a Fase 8

Com a entrega em produção ou integrada à branch principal, a esteira fecha o ciclo no estágio contínuo da **Fase 8: Governança, Memória Contínua & Aprendizado**:
* Persistência de contexto entre sessões de trabalho: [`unified-memory`](file:///home/willy/Sandbox/ecc/.agents/skills/unified-memory/SKILL.md) e [`ck`](file:///home/willy/Sandbox/ecc/.agents/skills/ck/SKILL.md)
* Extração e evolução de aprendizados em instintos permanentes: [`continuous-learning-v2`](file:///home/willy/Sandbox/ecc/.agents/skills/continuous-learning-v2/SKILL.md)
* Governança contra o apodrecimento da documentação: [`living-docs-governance`](file:///home/willy/Sandbox/ecc/.agents/skills/living-docs-governance/SKILL.md) e [`doc-updater`](file:///home/willy/Sandbox/ecc/.agents/agents/doc-updater.md)
