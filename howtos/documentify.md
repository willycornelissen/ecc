# Guia Prático: Documentando Projetos de Código com o ECC (Documentify)

Este guia estabelece o **passo a passo canônico e a melhor trilha de engenharia** para transformar qualquer projeto de código em um ecossistema com **documentação viva, autoexplicativa e visualmente rica**, utilizando Codemaps, Diagramas Mermaid nativos, ilustrações e governança contínua com o **ECC (*Everything Claude Code*)**.

---

## 1. Por Que a Maioria das Documentações Falha e a Abordagem do ECC

Documentar software é historicamente um dos maiores desafios da engenharia devido a três problemas recorrentes:

1. **Apodrecimento Precoce (*Doc Rot*)**: Documentos estáticos e páginas em wikis externas (Confluence, Notion) descolam-se da realidade do repositório semanas após serem criados. O README descreve comandos antigos, e a documentação de API omite campos novos.
2. **Paredes de Texto Monótonas (*Wall of Text*)**: Documentações extensas sem recursos visuais não são lidas por humanos e sobrecarregam a janela de contexto de agentes de IA com informações difusas.
3. **Falta de uma Fonte Única da Verdade**: A mesma informação sobre um endpoint ou banco de dados é duplicada em 3 ou 4 arquivos diferentes, criando contradições no primeiro refactoring.

### A Filosofia do ECC: Documentação Viva e Visual

No ECC, a documentação é tratada com o mesmo rigor do código-fonte:
* **Extraída Diretamente das Fontes da Verdade (*Sources of Truth*)**: Em vez de redigir tabelas manualmente, a documentação é gerada a partir de manifestos (`package.json`, `Cargo.toml`), esquemas de banco, rotas e arquivos `.env.example`.
* **Codemaps Otimizados para Token (*Token-Lean*)**: Mapas de código concisos estruturados para consumo imediato tanto por humanos quanto por agentes de IA.
* **Diagramas Nativos em Markdown (Mermaid)**: Diagramas que residem como código versionável no Git, renderizam nativamente no GitHub/GitLab e podem ser auditados em diffs de Pull Requests.
* **Governança de Quatro Papéis**: O princípio inegociável de *"um único dono canônico por fato"*, impedindo a duplicação de informações.

---

## 2. A Trilha Canônica de Documentação (Visão Geral)

O processo de estruturar a documentação de um projeto do zero ou de legado segue 7 fases integradas:

```mermaid
flowchart TD
    D0["Fase 0: Inventário das Fontes de Verdade<br/><i>(scripts, schemas, rotas, envs, docker)</i>"]
    D1["Fase 1: Mapeamento Arquitetural & Codemaps<br/><i>(subagente doc-updater / update-codemaps.md)</i>"]
    D2["Fase 2: Diagramação Visual Nativa (Mermaid & Desenhos)<br/><i>(Flowcharts, Sequence, ER, State & Arquitetura C4)</i>"]
    D3["Fase 3: Sincronização de Guias Técnicos<br/><i>(update-docs.md: CONTRIBUTING, RUNBOOK, ENV)</i>"]
    D4["Fase 4: Roteiros Guiados de Código (CodeTour)<br/><i>(code-tour / walkthroughs ancorados em arquivos)</i>"]
    D5["Fase 5: Higienização de Comentários no Código<br/><i>(subagente comment-analyzer / detecção de doc rot)</i>"]
    D6["Fase 6: Governança Contínua & Zona de Deleção<br/><i>(living-docs-governance / prevenção de drift)</i>"]

    D0 --> D1 --> D2 --> D3 --> D4 --> D5 --> D6
```

---

## 3. Os Quatro Papéis da Documentação Viva

Antes de criar qualquer arquivo, o ECC organiza a documentação em **quatro papéis sem sobreposição** ([`living-docs-governance`](file:///home/willy/Dablio/ecc/.agents/skills/living-docs-governance/SKILL.md)):

| Papel | Responsabilidade Única | Arquivos Canônicos Típicos | O que NUNCA deve conter |
| :--- | :--- | :--- | :--- |
| **Constituição** | Regras que humanos e agentes devem obedecer obrigatoriamente. | `CLAUDE.md`, `AGENTS.md`, `.ai-style-rules.md`, `CONTRIBUTING.md` | Status temporário ou tutoriais longos. |
| **Mapa** | O que existe no sistema, onde reside, fronteiras de domínio e pontos de entrada. | `docs/CODEMAPS/*`, `docs/architecture.md`, `README.md` | Logs de incidentes ou planos de tarefas futuras. |
| **Status** | Saúde atual do projeto, metas do sprint atual e a **Zona de Deleção** (*Delete Zone*). | `docs/STATUS.md`, `docs/ROADMAP.md` | Referências estruturais permanentes. |
| **Histórico** | Registro de decisões arquiteturais duráveis, substituições e incidentes materiais. | `docs/adr/NNNN-*.md`, `CHANGELOG.md` | Cópia trivial de commits do Git. |

> [!TIP]
> **A Regra de Ouro da Governança**: Cada fato tem **apenas um proprietário**. Outros documentos apenas apontam links em vez de copiar o texto.
> * "Onde fica a camada de autenticação?" $\rightarrow$ Pertence ao **Mapa**.
> * "Quais as regras inegociáveis de segurança?" $\rightarrow$ Pertence à **Constituição**.
> * "A migração do banco foi concluída?" $\rightarrow$ Pertence ao **Status**.
> * "Por que migramos de REST para gRPC?" $\rightarrow$ Pertence ao **Histórico** (ADR).

---

## 4. Passo a Passo Detalhado (Runbook Documentify)

### Fase 0: Inventário das Fontes de Verdade (*Sources of Truth*)

Não invente documentação a partir da imaginação. Localize onde a verdade real do sistema já está expressa em código:

1. **Comandos e Tooling**: Inspecione `package.json` (scripts), `Makefile`, `Cargo.toml`, `pyproject.toml` ou `go.mod`.
2. **Ambiente e Configurações**: Inspecione `.env.example`, `.env.template` ou configurações de container.
3. **Contratos de Interface e APIs**: Inspecione rotas HTTP, controladores, schemas Zod/Pydantic ou definições OpenAPI/Swagger.
4. **Modelo de Dados**: Inspecione schemas do ORM (`schema.prisma`, models SQLAlchemy, migrations SQL, schemas Drizzle).
5. **Infraestrutura**: Inspecione `Dockerfile`, `docker-compose.yml`, manifests Kubernetes ou arquivos Terraform.

---

### Fase 1: Mapeamento Arquitetural & Codemaps Token-Lean

Os **Codemaps** são representações de arquitetura ultra-concisas, desenhadas para orientar desenvolvedores e fornecer contexto instantâneo para agentes de IA sem desperdício de tokens.

#### 1. Gerar Codemaps com o Subagente [`doc-updater`](file:///home/willy/Dablio/ecc/.agents/agents/doc-updater.md)
Execute o workflow dedicado:
```text
/update-codemaps
```
O subagente [`doc-updater`](file:///home/willy/Dablio/ecc/.agents/agents/doc-updater.md) varre a árvore do repositório e gera a estrutura em `docs/CODEMAPS/`:

* **`INDEX.md`**: Visão executiva da topologia de pastas, tecnologias principais e convenções.
* **`architecture.md`**: Fronteiras entre módulos, fluxos de dados e integrações externas.
* **`backend.md`**: Mapeamento linear de rotas $\rightarrow$ controllers $\rightarrow$ services $\rightarrow$ repositories.
* **`frontend.md`**: Hierarquia de páginas, árvore de componentes e fluxo de gerenciamento de estado.
* **`data.md`**: Diagrama relacional, tabelas principais, chaves estrangeiras e histórico de migrations.
* **`dependencies.md`**: Serviços de terceiros (Stripe, AWS S3, Redis, Sendgrid) e SDKs essenciais.

#### 2. Formato Exemplo de um Codemap Token-Lean (`docs/CODEMAPS/backend.md`)
```markdown
# Backend Codemap

## Cadeia de Execução de Rotas
POST /api/auth/login    → AuthController.login    → AuthService.validate   → UserRepo.findByEmail
POST /api/orders        → OrderController.create   → OrderService.checkout  → PaymentGateway.charge
GET  /api/orders/:id    → OrderController.getById  → OrderService.find      → OrderRepo.findById

## Módulos Críticos
- `src/services/order.ts` (Lógica de checkout e cálculo de frete, 180 linhas)
- `src/infra/database.ts` (Pool de conexão PostgreSQL e transações, 65 linhas)

## Serviços Externos Vinculados
- Redis (Cache de sessão e rate limiting)
- Stripe SDK (Processamento de cartões de crédito)
```

---

### Fase 2: Diagramação Visual Nativa (Mermaid & Desenhos)

A documentação moderna não deve depender de capturas de tela desatualizadas ou ferramentas proprietárias fechadas. Use **código Markdown com Mermaid** para renderização nativa versionada no repositório.

#### Qual Tipo de Diagrama Usar em Cada Caso?

| Necessidade Arquitetural | Tipo de Diagrama Recomendado | Exemplo Prático |
| :--- | :--- | :--- |
| **Topologia Geral do Sistema** | `flowchart TD` ou `flowchart LR` | Serviços, bancos, filas e navegadores. |
| **Fluxo no Tempo / Requisições** | `sequenceDiagram` | Autenticação OAuth2, webhook assíncrono. |
| **Modelagem de Banco de Dados** | `erDiagram` | Tabelas, relacionamentos (1:N, N:N) e colunas. |
| **Ciclo de Vida de Entidades** | `stateDiagram-v2` | Estados de um Pedido, Fatura ou Tarefa. |
| **Domínio & Herança/Composição** | `classDiagram` | Entidades DDD, interfaces e factories. |

---

#### 1. Diagrama de Arquitetura de Alto Nível (Flowchart)

```mermaid
flowchart TD
    subgraph ClientLayer["Camada de Apresentação (Frontend)"]
        SPA["Next.js SPA / Web App"]
        Mobile["Mobile Client (React Native)"]
    end

    subgraph GatewayLayer["Borda e Roteamento"]
        Gateway["API Gateway / Nginx Reverse Proxy"]
        Auth["Auth Guard (JWT Middleware)"]
    end

    subgraph ServicesLayer["Serviços de Domínio"]
        UserService["User & Account Service"]
        OrderService["Order Processing Service"]
        PaymentWorker["Async Payment Worker"]
    end

    subgraph DataLayer["Persistência & Mensageria"]
        Postgres[("PostgreSQL (Primary DB)")]
        RedisCache[("Redis (Session & Cache)")]
        RabbitMQ{{"RabbitMQ (Event Bus)"}}
    end

    SPA -->|HTTPS / REST| Gateway
    Mobile -->|HTTPS / REST| Gateway
    Gateway --> Auth
    Auth --> UserService
    Auth --> OrderService
    OrderService --> RabbitMQ
    RabbitMQ --> PaymentWorker
    UserService --> Postgres
    OrderService --> Postgres
    PaymentWorker --> Postgres
    Gateway --> RedisCache
```

---

#### 2. Diagrama de Sequência para Fluxos Críticos (`sequenceDiagram`)

Excelente para documentar interações entre múltiplos serviços, APIs de terceiros ou middlewares assíncronos:

```mermaid
sequenceDiagram
    autonumber
    actor User as Usuário Final
    participant Browser as Navegador (SPA)
    participant API as API Server
    participant Auth0 as Provedor OAuth2
    participant DB as PostgreSQL

    User->>Browser: Clica em "Login com Google"
    Browser->>Auth0: Redireciona para tela de consentimento
    Auth0-->>Browser: Retorna Authorization Code
    Browser->>API: POST /api/auth/callback (code)
    API->>Auth0: Troca Code por Access Token
    Auth0-->>API: Retorna Token + Perfil do Usuário
    API->>DB: SELECT / INSERT em `users`
    DB-->>API: Confirma registro
    API-->>Browser: Retorna HTTP 200 + Session Cookie (HttpOnly)
    Browser-->>User: Exibe Dashboard autenticado
```

---

#### 3. Diagrama Entidade-Relacionamento (`erDiagram`)

Para documentar esquemas de dados relacionais diretamente das migrations do projeto:

```mermaid
erDiagram
    ORGANIZATION ||--o{ USER : contains
    ORGANIZATION ||--o{ PROJECT : owns
    USER ||--o{ TASK : assigned_to
    PROJECT ||--o{ TASK : includes
    TASK ||--o{ COMMENT : has

    ORGANIZATION {
        uuid id PK
        string name
        timestamp created_at
    }

    USER {
        uuid id PK
        uuid org_id FK
        string email
        string role
    }

    PROJECT {
        uuid id PK
        uuid org_id FK
        string title
        string status
    }

    TASK {
        uuid id PK
        uuid project_id FK
        uuid user_id FK
        string title
        string priority
    }

    COMMENT {
        uuid id PK
        uuid task_id FK
        text content
    }
```

---

#### 4. Diagrama de Máquina de Estados de Negócio (`stateDiagram-v2`)

Essencial para explicitar regras de negócio de workflows sem ambiguidade:

```mermaid
stateDiagram-v2
    [*] --> Rascunho : Criado pelo usuário
    Rascunho --> EmRevisao : Submetido para aprovação
    EmRevisao --> Rascunho : Rejeitado com apontamentos
    EmRevisao --> Aprovado : Aprovado pelo gestor
    Aprovado --> EmProcessamento : Disparado para fila
    EmProcessamento --> Concluido : Pagamento liquidado
    EmProcessamento --> Falha : Erro na operadora
    Falha --> EmProcessamento : Retry automático (até 3x)
    Falha --> Cancelado : Excedeu retentativas
    Concluido --> [*]
    Cancelado --> [*]
```

---

#### Quando Utilizar Desenhos e Imagens Complementares?

Diagramas de código em Mermaid cobrem 90% das necessidades técnicas. No entanto, quando forem necessários **desenhos conceituais de alto nível, wireframes visuais de UI ou ilustrações de apresentação**, o ECC oferece suporte a:

1. **Subagente `image-generator`**: Pode ser acionado para criar ilustrações conceituais ou diagramas pictóricos a partir de briefings, salvando os assets em `docs/assets/images/`.
2. **SVGs Vetoriais Embutidos**: Gráficos e ícones vetoriais leves incluídos diretamente nos arquivos Markdown para visualização responsiva.
3. **Diagramas no Plan Canvas**: Abertura interativa de planos e fluxos técnicos no navegador via [`plan-canvas`](file:///home/willy/Dablio/ecc/.agents/skills/plan-canvas/SKILL.md) para anotação visual pelo time.

---

### Fase 3: Sincronização de Guias Técnicos com [`update-docs.md`](file:///home/willy/Dablio/ecc/.agents/workflows/update-docs.md)

Execute o workflow canônico de sincronização:
```text
/update-docs
```
Ele atualiza os seguintes arquivos essenciais:

#### 1. Dicionário de Variáveis de Ambiente (`docs/ENVIRONMENT.md`)
Gera a tabela estruturada a partir de `.env.example`:

| Variável | Obrigatória | Tipo / Padrão | Descrição | Exemplo de Valor |
| :--- | :--- | :--- | :--- | :--- |
| `DATABASE_URL` | **Sim** | `string` (URI) | String de conexão com PostgreSQL | `postgres://user:pass@localhost:5432/app` |
| `REDIS_URL` | **Sim** | `string` (URI) | Instância Redis para cache e rate limit | `redis://localhost:6379` |
| `PORT` | Não | `number` (3000) | Porta local de escuta do servidor HTTP | `8080` |
| `LOG_LEVEL` | Não | `info` | Nível de detalhamento dos logs de produção | `debug`, `info`, `warn`, `error` |

#### 2. Guia de Contribuição e Scripts (`docs/CONTRIBUTING.md`)
Mapeia comandos disponíveis para desenvolvedores:
* Inicialização do ambiente local com Docker.
* Execução da suíte de testes e linters.
* Convenções de branch e formato obrigatório de commits (*Conventional Commits*).

#### 3. Runbook Operacional de Produção (`docs/RUNBOOK.md`)
* Procedimentos passo a passo de deploy e rollback.
* Endpoints de health check (`/healthz`, `/metrics`).
* Planos de contingência para falhas de banco ou degradação de filas.

---

### Fase 4: Roteiros Guiados de Código com [`code-tour`](file:///home/willy/Dablio/ecc/.agents/skills/code-tour/SKILL.md)

Textos estáticos frequentemente falham em mostrar **onde o código realmente executa**. O ECC resolve isso através da skill [`code-tour`](file:///home/willy/Dablio/ecc/.agents/skills/code-tour/SKILL.md), criando arquivos `.tour` navegáveis dentro do VS Code / Cursor / IDEs compatíveis.

#### 1. Gerar Roteiros Navegáveis
Execute:
```text
"Criar um code tour com o fluxo de autenticação e requisição de pedidos usando a skill code-tour"
```
A skill cria arquivos JSON estruturados em `.tours/` contendo:
* O arquivo exato e o intervalo de linhas (`file` e `line`).
* O título do passo na jornada.
* A explicação conceitual de por que aquele bloco de código existe.
* Para qual arquivo navegar a seguir na cadeia de execução.

**Exemplo de Tours Práticos a Manter no Repositório**:
* `.tours/onboarding-primeiros-passos.tour`: Guia para novos desenvolvedores entenderem o bootstrap da aplicação.
* `.tours/trilha-critica-pagamento.tour`: Caminho percorrido por uma transação financeira do controller ao webhook da adquirente.

---

### Fase 5: Higienização de Comentários no Código com [`comment-analyzer`](file:///home/willy/Dablio/ecc/.agents/agents/comment-analyzer.md)

Comentários mentirosos ou obsoletos são piores do que a ausência de comentários.

#### 1. Varredura com o Subagente [`comment-analyzer`](file:///home/willy/Dablio/ecc/.agents/agents/comment-analyzer.md)
Invoque o [`comment-analyzer`](file:///home/willy/Dablio/ecc/.agents/agents/comment-analyzer.md) para auditar os comentários do código-fonte:
* **Inaccurate (Incorretos)**: Comentários que descrevem parâmetros ou comportamentos alterados em refatorações passadas.
* **Stale References (Referências Obsoletas)**: Comentários que citam funções, classes ou endpoints que já foram deletados.
* **Low-Value (Baixo Valor)**: Comentários óbvios que apenas repetem a sintaxe (ex: `// incrementa o contador i++`).
* **Dívida Técnica de TODOs**: Catalogação de `// TODO`, `// FIXME` e `// HACK` esquecidos há meses.

---

### Fase 6: Governança Contínua & Zona de Deleção (*Anti-Doc-Rot*)

Documentação não é uma tarefa única, mas um hábito mantido por regras mecânicas.

#### 1. A Zona de Deleção (*Delete Zone*)
Um dos maiores problemas em projetos que usam agentes de IA é o modelo "tentar recriar" arquivos ou módulos intencionalmente deletados porque ele assume que estão faltando.
* Documente explicitamente em `docs/STATUS.md` ou `docs/DEPRECATIONS.md`:
  ```markdown
  ## Zona de Deleção (Arquivos Removidos Intencionalmente)
  - `src/legacy/xml-parser.ts`: Removido em favor do parser JSON nativo (ADR-0012). PROIBIDO recriar.
  - `src/utils/md5.ts`: Removido por vulnerabilidade criptográfica. Usar `bcrypt` ou `argon2`.
  ```

#### 2. Travas de Pré-Merge / PR
* Garanta que o workflow [`pr.md`](file:///home/willy/Dablio/ecc/.agents/workflows/pr.md) exija que qualquer alteração de rotas ou modelos venha acompanhada da atualização correspondente em `docs/CODEMAPS/`.

---

## 5. Estrutura Canônica do Diretório `docs/` no ECC

Após a execução da esteira Documentify, o repositório deve apresentar a seguinte organização limpa e modular:

```text
meu-projeto/
├── CLAUDE.md                    # Constituição: Comandos e regras canônicas do harness
├── README.md                     # Portal de Entrada: Visão geral, badges e diagrama principal
├── .tours/                       # CodeTours interativos para a IDE
│   ├── onboarding.tour
│   └── fluxo-pedidos.tour
└── docs/
    ├── CODEMAPS/                 # Mapas de código Token-Lean para humanos e IAs
    │   ├── INDEX.md              # Índice e topologia
    │   ├── architecture.md       # Diagrama de blocos e fronteiras
    │   ├── backend.md            # Rotas, controllers, services
    │   ├── frontend.md           # Componentes e estado
    │   ├── data.md               # Modelo relacional e migrations
    │   └── dependencies.md       # Serviços externos e SDKs
    ├── adr/                      # Histórico de Decisões de Arquitetura (ADRs)
    │   ├── 0001-stack-inicial.md
    │   └── 0002-banco-de-dados.md
    ├── CONTRIBUTING.md           # Guia de desenvolvimento e padrões de PR
    ├── ENVIRONMENT.md            # Dicionário de variáveis de ambiente
    ├── RUNBOOK.md                # Operações de deploy, monitoramento e contingência
    └── STATUS.md                 # Saúde atual do projeto e Zona de Deleção
```

---

## 6. Anti-padrões Críticos na Documentação de Software

| Anti-padrão Clássico | Por que é Catastrófico? | Como o ECC Resolve? |
| :--- | :--- | :--- |
| **Diagramas em PNG estáticos desatualizados** | Ninguém edita o arquivo de imagem; 2 meses depois o diagrama mente sobre a arquitetura. | **Diagramas Mermaid nativos em Markdown**: O diagrama é código versionado no Git e editado em diffs de PR. |
| **Duplicação de Regras em Múltiplos Arquivos** | Uma regra é atualizada no README mas continua desatualizada no `CONTRIBUTING.md`. | **Governança de 4 Papéis**: Apenas um dono canônico por fato. Outros arquivos usam links. |
| **Documentação "Enciclopédica" Gigante** | Textos prolixos com 40 páginas que nenhum dev lê e que estouram a janela de contexto de agentes. | **Codemaps Token-Lean**: Resumos atômicos estruturados em tabelas e bullet points cirúrgicos. |
| **Manter Comentários Obsoletos no Código** | Desenvolvedores e agentes confiam no comentário falso e introduzem bugs silenciosos. | Varredura e higienização contínua com o subagente [`comment-analyzer`](file:///home/willy/Dablio/ecc/.agents/agents/comment-analyzer.md). |
| **Ignorar Módulos Excluídos** | Agentes de IA tentam recriar código antigo achando que foi perdido por engano. | **Zona de Deleção**: Manter o registro formal dos módulos intencionalmente aposentados. |

---

## 7. Checklist Operacional de Documentação (Documentify Checklist)

Use este checklist prático para auditar a completude da documentação do seu projeto:

- [ ] **Topologia e Codemaps**
  - [ ] Diretório `docs/CODEMAPS/` gerado via `/update-codemaps`.
  - [ ] Diagrama de blocos de alto nível em Mermaid renderizando no `docs/CODEMAPS/architecture.md`.
  - [ ] Mapeamento linear de rotas e services registrado em `backend.md`.
  - [ ] Diagrama Entidade-Relacionamento (ER) atualizado em `data.md`.
- [ ] **Guias e Referências Técnicas**
  - [ ] Dicionário de variáveis de ambiente categorizado (`docs/ENVIRONMENT.md`).
  - [ ] Comandos canônicos de build, teste e lint formalizados em `CLAUDE.md`.
  - [ ] Guia de contribuição e convenções de commit em `docs/CONTRIBUTING.md`.
  - [ ] Procedimentos de deploy, health check e contingência em `docs/RUNBOOK.md`.
- [ ] **Recursos Visuais & Roteiros**
  - [ ] Diagramas Mermaid de sequência documentando fluxos críticos e assíncronos.
  - [ ] Pelo menos um CodeTour navegável configurado em `.tours/` para onboarding de novatos.
- [ ] **Governança e Limpeza**
  - [ ] Comentários no código varridos e higienizados (`comment-analyzer`).
  - [ ] Zona de Deleção registrada em `docs/STATUS.md` com módulos proibidos de recriar.
  - [ ] Decisões estruturais registradas em ADRs numeradas em `docs/adr/`.
