O **ECC** (nascido como *Everything Claude Code* e hoje adotando oficialmente apenas a sigla **ECC** após expandir seu suporte para múltiplos ambientes) é um **sistema operacional e kit de ferramentas de engenharia para *agent harnesses*** (ambientes de execução de agentes de código), criado por Affaan Mustafa sob licença MIT.

Enquanto um assistente de código comum apenas responde a prompts isolados, o ECC fornece uma camada operacional padronizada que força o agente a trabalhar dentro de um ciclo de engenharia disciplinado: **planejar → testar → implementar → revisar → verificar → memorizar → melhorar**.

---

## O que é e como funciona a arquitetura do ECC

O lema central do projeto é *"Otimize a janela de contexto. Persista todo o resto"*. Em vez de você repetir instruções arquiteturais, regras de negócio e padrões de teste a cada novo prompt, o ECC instala uma estrutura modular diretamente no seu repositório ou ambiente global.

Os pilares que compõem o ecossistema incluem:

* **Catálogo Massivo de Especialistas (71 Agents e 311 Skills):** Agentes com escopo estrito (arquitetura, revisão de código em contexto limpo, reparo de build, segurança) e *skills* acionáveis sob demanda (TDD, padrões de TypeScript/React, bancos de dados, documentação, pesquisa prévia).
* **Hooks, Rules e "Instincts":** Ganchos de tempo de execução (*runtime hooks*) que interceptam ações do agente para impor validações, controlar o orçamento da janela de contexto (avisando antes da compactação nativa degradar a memória da sessão) e transformar acertos recorrentes em lições aprendidas (*continuous learning*).
* **Unified Memory (Memória Unificada e Handoff):** Permite salvar o estado e o contexto de uma tarefa em um agente e retomá-la em outro sem perda de histórico. Por exemplo, você pode iniciar o planejamento no Codex e transferir a execução para o Claude Code ou OpenCode via CLI/MCP (`ecc memory handoff`).
* **Plan Canvas & Control Pane:** Interfaces locais onde você visualiza o plano do agente no navegador, faz anotações pontuais nas partes que deseja alterar antes da escrita de código e monitora múltiplas sessões ou *git worktrees* operando em paralelo.
* **AgentShield:** Um scanner de segurança dedicado à superfície do próprio agente. Ele audita configurações, permissões excessivas, ganchos maliciosos, vulnerabilidades de *prompt injection*, segredos expostos e conexões MCP inseguras.
* **Adaptadores Multi-Harness (`ecc-universal`):** Embora tenha suporte nativo primário no Claude Code e Codex, o ECC possui adaptadores gerenciados para **OpenCode**, **Cursor**, **Antigravity**, **Gemini**, **Zed** e **Kimi**.

---

## Quando e para que usar

O ECC não é necessário para scripts rápidos ou dúvidas pontuais de sintaxe, mas torna-se um multiplicador de força em cenários específicos:

1. **Orquestração entre múltiplas ferramentas (Multi-Harness):** Quando seu fluxo de trabalho alterna entre diferentes CLIs e IDEs (como usar Cursor no editor visual, mas rodar agentes autônomos via terminal no OpenCode, Antigravity ou Claude Code) e você deseja que todos compartilhem exatamente as mesmas regras, *skills* e memória de sessão.
2. **Projetos de médio a grande porte com alto rigor técnico:** Quando o "vibe coding" (pedir código no improviso) começa a gerar dívida técnica, regressões silenciosas ou alucinações por estourar a janela de contexto.
3. **Revisão em Contexto Limpo (Fresh-Context Review):** Quando o agente que escreveu o código fica "viciado" nas próprias premissas, o ECC aciona um subagente revisor em um contexto isolado apenas para auditar o *diff* gerado.
4. **Governança e Segurança em Equipes:** Para padronizar como os desenvolvedores interagem com IAs no repositório (via *ECC GitHub App* ou configurações versionadas) e auditar servidores MCP com o *AgentShield*.

> **Atenção ao custo de contexto:** Instalar todas as 311 *skills* e dezenas de regras de uma só vez consome tokens preciosos da janela de contexto. A melhor prática no ECC é a **instalação seletiva** (`npx ecc-universal install --guided`), ativando apenas as regras da sua stack (ex.: TypeScript, Next.js, Python) e carregando *skills* sob demanda.

---

## Diferenças para outras metodologias de desenvolvimento assistido por IA

Hoje existem diferentes paradigmas para estruturar o desenvolvimento com IA. A tabela abaixo compara o **ECC** com as principais abordagens do mercado:

| Critério | Vibe Coding / Prompting Ad-hoc | Regras Estáticas (`.cursorrules`, `CLAUDE.md` puro) | Spec-Driven Development (BMAD, Taskmaster, SpecKit) | Frameworks Multi-Agentes (CrewAI, AutoGen, LangGraph) | **ECC (Everything Claude Code)** |
| --- | --- | --- | --- | --- | --- |
| **Foco Principal** | Velocidade imediata por tentativa e erro | Alinhamento passivo de estilo e convenções | Documentação exaustiva (PRDs/Specs) antes do código | Construção de produtos/backends baseados em agentes via código | **Otimização da engenharia, memória, segurança e runtime do agente** |
| **Como opera** | Conversa livre no chat | Injeção de texto fixo no *system prompt* | Cascata rígida de artefatos (PM → Arquiteto → Dev) | Orquestração programática via APIs em Python/TS | **Camada de SO sobre o *harness* com hooks, skills, memória e CLI** |
| **Gestão de Contexto** | Degrada rápido em sessões longas | Ocupa espaço fixo; sem controle de compactação | Isolado por arquivos de tarefas/épicos | Controlado via código pelo desenvolvedor | **Orçamento ativo de contexto, alertas de compactação e handoffs** |
| **Portabilidade** | Nenhuma (presa ao histórico do chat) | Presa a uma ferramenta específica (ou exige cópia manual) | Portável via arquivos Markdown no repo | Independente de IDE (roda como aplicação separada) | **Nativa multi-harness (Claude, Cursor, OpenCode, Antigravity, Codex)** |
| **Segurança do Agente** | Inexistente | Depende de regras textuais que o LLM pode ignorar | Foco em requisitos de negócio, não na superfície do agente | Guardrails de aplicação | **Auditoria ativa de permissões, hooks e MCP via AgentShield** |
| **Aprendizado Contínuo** | Zero entre sessões | Manual (você edita o arquivo quando lembra) | Manual (atualização de specs) | Via bancos vetoriais externos (RAG) | **Sistema de *Instincts* que extrai padrões de sucesso e vira regra/skill** |

### Resumo das distinções práticas:

* **ECC vs. Arquivos de Regras (`.cursorrules` / `AGENTS.md` puros):** Arquivos estáticos são apenas texto passivo; o modelo pode simplesmente ignorá-los quando o contexto cresce. O ECC usa **hooks executáveis** que travam ações indevidas, forçam a passagem por testes antes de concluir uma tarefa e gerenciam a memória ativamente.
* **ECC vs. Metodologias Baseadas em Especificação (como BMAD ou Taskmaster):** Metodologias *Spec-Driven* focam em simular uma fábrica de software burocrática (gerando longos documentos de Produto e Arquitetura antes de tocar no código). O ECC é mais **pragmático e centrado em engenharia de código**: ele tem o *Plan Canvas* para planejar rápido, mas seu diferencial real está no tempo de execução (TDD, revisão cruzada, reparo de build, segurança de MCP e transferência de memória entre ferramentas).