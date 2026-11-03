# Skill: E2E Testing (`e2e-testing`)

A skill **E2E Testing** é o guia de padrões arquiteturais e boas práticas para testes ponta a ponta (*End-to-End*) utilizando **Playwright** no ecossistema **ECC (Everything Claude Code)**. Ela atua como o topo da pirâmide de testes na [Fase 4: Implementação Guiada por Testes (TDD)](file:///home/willycornelissen/Desktop/Dablio/ecc/markdown/Fase%204%20-%20Implementação%20Guiada%20por%20Testes%20(TDD).md) e prepara a base para a validação visual na [Fase 6: E2E & Auditoria de UI](file:///home/willycornelissen/Desktop/Dablio/ecc/markdown/Fase%206%20-%20Testes%20Ponta%20a%20Ponta%20(E2E)%20&%20Auditoria%20de%20UI.md).

Seu lema de engenharia é:
> **"Testes E2E devem validar as jornadas críticas do usuário como um navegador real as experimenta, com execução paralela rápida, isolamento via Page Objects e tolerância zero a testes instáveis (*flaky tests*) no CI/CD."**

---

## 🎯 Por que a Skill Existe?

Testes E2E são notoriamente difíceis de manter:
- Seletores frágeis (baseados em classes CSS ou XPaths voláteis) quebram com qualquer ajuste cosmético de layout.
- Condições de corrida de rede (*race conditions*) causam falhas intermitentes (*flakiness*) em pipelines de CI/CD.
- Falta de Page Objects resulta em duplicação massiva de código de automação.

A skill `e2e-testing` padroniza a infraestrutura de testes no Playwright, impondo boas práticas de locators, quarentena de testes instáveis e gestão automática de evidências (vídeos, screenshots e traces).

---

## 📁 Organização Recomendada de Pastas

```
tests/
├── e2e/
│   ├── auth/                  ← Fluxos de login, logout e cadastro
│   │   ├── login.spec.ts
│   │   └── register.spec.ts
│   ├── features/              ← Jornadas críticas de negócio (User Journeys)
│   │   ├── checkout.spec.ts
│   │   └── search.spec.ts
│   └── pages/                 ← Classes do Page Object Model (POM)
│       ├── BasePage.ts
│       ├── LoginPage.ts
│       └── CheckoutPage.ts
├── fixtures/                  ← Estados pré-carregados (sessão auth, tokens)
│   ├── auth.ts
│   └── test-data.ts
└── playwright.config.ts       ← Configuração unificada do Playwright
```

---

## 🧱 Padrão Page Object Model (POM)

O Page Object Model encapsula a lógica de interação e os seletores de uma tela em uma classe dedicada, mantendo os arquivos `.spec.ts` limpos e legíveis:

```typescript
// tests/e2e/pages/ItemsPage.ts
import { Page, Locator } from '@playwright/test';

export class ItemsPage {
  readonly page: Page;
  readonly searchInput: Locator;
  readonly itemCards: Locator;
  readonly createButton: Locator;

  constructor(page: Page) {
    this.page = page;
    // Seletores sempre orientados a atributos de teste estáveis (data-testid)
    this.searchInput = page.locator('[data-testid="search-input"]');
    this.itemCards = page.locator('[data-testid="item-card"]');
    this.createButton = page.locator('[data-testid="create-btn"]');
  }

  async goto() {
    await this.page.goto('/items');
    await this.page.waitForLoadState('networkidle');
  }

  async search(query: string) {
    await this.searchInput.fill(query);
    // Aguarda a resposta real da API de busca, e não um sleep arbitrário
    await this.page.waitForResponse(resp => resp.url().includes('/api/search'));
  }

  async getItemCount() {
    return await this.itemCards.count();
  }
}
```

### O Teste Consumidor do Page Object:

```typescript
// tests/e2e/features/search.spec.ts
import { test, expect } from '@playwright/test';
import { ItemsPage } from '../pages/ItemsPage';

test.describe('Jornada de Busca de Itens', () => {
  let itemsPage: ItemsPage;

  test.beforeEach(async ({ page }) => {
    itemsPage = new ItemsPage(page);
    await itemsPage.goto();
  });

  test('deve filtrar itens por palavra-chave com sucesso', async () => {
    await itemsPage.search('notebook');
    const count = await itemsPage.getItemCount();
    expect(count).toBeGreaterThan(0);
    await expect(itemsPage.itemCards.first()).toContainText(/notebook/i);
  });
});
```

---

## ⚡ Estratégias contra Testes Instáveis (*Flaky Tests*)

Testes instáveis destroem a confiança da equipe na esteira de CI. A skill prescreve quatro regras de ouro:

### 1. Eliminação de Esperas Arbitrárias (`sleep` / `waitForTimeout`)
* ❌ **Proibido**: `await page.waitForTimeout(5000);` (deixa a suíte lenta e quebra se o CI estiver sobrecarregado).
*  **Correto**: Aguardar a condição observável exata:
  ```typescript
  await page.waitForResponse(resp => resp.url().includes('/api/pedidos') && resp.status() === 200);
  ```

### 2. Uso dos Locators com Auto-Wait Nativo
O Playwright aguarda automaticamente elementos ficarem visíveis, habilitados e estáveis antes de clicar:
* ❌ **Frágil**: `await page.click('#meu-botao');`
*  **Estável**: `await page.locator('[data-testid="confirmar-btn"]').click();`

### 3. Padrão de Quarentena (*Test Quarantine*)
Quando um teste apresentar intermitência em produção, ele não deve ser deletado, mas sim colocado em quarentena documentada:
```typescript
test('fluxo complexo com webhook externo', async ({ page }) => {
  test.fixme(true, 'Instável no CI sob alta concorrência - Issue #342');
  // código do teste...
});
```

### 4. Diagnóstico de Repetição
Comando para estressar e provar se um teste é instável antes de mesclar:
```bash
npx playwright test tests/e2e/checkout.spec.ts --repeat-each=10
```

---

## 📸 Gestão Automática de Evidências e Artefatos

No arquivo `playwright.config.ts`, a skill configura a retenção inteligente de mídias para depuração:

* **Screenshots apenas em caso de falha**: `screenshot: 'only-on-failure'`
* **Gravação de vídeo em falhas**: `video: 'retain-on-failure'`
* **Traces do Playwright na primeira retentativa**: `trace: 'on-first-retry'` (permite inspecionar a linha do tempo exata com o Trace Viewer: `npx playwright show-trace artifacts/trace.zip`).

---

## 🔄 Pipeline CI/CD no GitHub Actions

A skill inclui o modelo otimizado para execução paralela em esteiras de integração contínua:

```yaml
# .github/workflows/e2e.yml
name: E2E Playwright Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
        env:
          BASE_URL: ${{ vars.STAGING_URL }}
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 30
```

---

## 🔗 Integração na Esteira ECC

1. **Fecha a Pirâmide de Testes da [Fase 4 (TDD)](file:///home/willycornelissen/Desktop/Dablio/ecc/skills/4.%20TDD/tdd-workflow.md)**: Valida as *User Journeys* de ponta a ponta após os testes unitários e de integração passarem.
2. **Executada pelo subagente [`e2e-runner`](file:///home/willycornelissen/Desktop/Dablio/ecc/.agents/agents/e2e-runner.md)**: Subagente especializado que orquestra a geração, manutenção e execução dos testes E2E.
3. **Prepara para a [Fase 6: E2E & Auditoria de UI](file:///home/willycornelissen/Desktop/Dablio/ecc/markdown/Fase%206%20-%20Testes%20Ponta%20a%20Ponta%20(E2E)%20&%20Auditoria%20de%20UI.md)**: Conecta-se à skill [`browser-qa`](file:///home/willycornelissen/Desktop/Dablio/ecc/.agents/skills/browser-qa/SKILL.md) para verificar auditorias de acessibilidade (axe-core), regressão visual em 3 breakpoints e validação de console errors.
