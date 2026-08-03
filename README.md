# Playwright CLI

## O que é?

Uma **interface de linha de comando** para automação de navegadores, projetada para agentes de programação. Comandos com uso eficiente de tokens e habilidades instaláveis permitem que os agentes equilibrem a automação de navegadores com grandes bases de código e o raciocínio dentro de janelas de contexto limitadas.

---

## Exemplos

$ playwright-cli open https://demo.playwright.dev/todomvc --headed
$ playwright-cli type "Buy groceries"
$ playwright-cli press Enter
$ playwright-cli type "Water flowers"
$ playwright-cli press Enter
$ playwright-cli check e21
$ playwright-cli screenshot

---

## Por que usar Playwright CLI?

Embora o MCP proporcione uma interação rica com o navegador, ele também introduz uma limitação crítica. Com o MCP, o agente recebe árvores de acessibilidade, esquemas de ferramentas, metadados da página e registros do console. Embora essa resposta seja extremamente rica, toda essa quantidade de dados sobrecarrega a janela de contexto da IA e reduz o espaço para raciocínio mais profundo e geração de código.

Agentes de codificação modernos operam sob limites rígidos de tokens. Para resolver esse problema, a Microsoft introduziu uma abordagem de linha de comando: em vez de exibir o estado completo da página, a CLI retorna **referências compactas** que representam cada elemento da página.

---

## Principais características

- Eficiência de tokens — a saída concisa da CLI evita o carregamento de grandes esquemas de ferramentas no contexto do modelo.
- Baseado em habilidades — os agentes descobrem funcionalidades por meio de habilidades instaláveis, em vez de textos de ajuda extensos.
- Arquitetura daemon — processo de navegador persistente significa que não há custo de inicialização por comando
- Baseado em referências — instantâneos de acessibilidade com referências de elementos para interação determinística.
- Compatível com vários navegadores — suporte para Chrome, Firefox, WebKit e Edge.
- Sessões — múltiplas instâncias isoladas do navegador com estados separados.

---

## Playwright CLI vs MCP

| | Playwright CLI | MCP |
| --- | --- | --- |
| **Ideal para** | Agentes de programação (Claude Code, Copilot) que trabalham com grandes bases de código | Loops agenticos especializados, automação exploratória |
| **Como funciona** | O agente executa comandos no shell | O LLM chama ferramentas MCP com parâmetros estruturados |
| **Custo em tokens** | Menor — saída concisa da CLI; skills carregadas sob demanda | Maior — schemas das ferramentas + snapshots no contexto |
| **Modo padrão** | Headless | Headed |
| **Configuração** | `npm install -g @playwright/cli` | Configuração JSON no cliente MCP |

---

## Como fazer

### 1. Ter um projeto Playwright pronto

Antes da CLI, é preciso de um projeto de testes Playwright. Se ainda não tiver um, crie assim:

```bash
npm init playwright@latest --yes -- --quiet --browser=chromium --browser=firefox --browser=webkit --gha
```

Isso gera o projeto com Chromium, Firefox, WebKit e um workflow de GitHub Actions.

### 2. Instalação

Com o projeto pronto, instale o `playwright-cli` ([documentação](https://playwright.dev/docs/getting-started-cli)).

**Instale globalmente:**

```bash
npm install -g @playwright/cli@latest
playwright-cli --help
```

**Alternativamente**, instale `@playwright/cli` como dependência local e use via `npx`:

```bash
npm install -D @playwright/cli@latest
npx playwright-cli --help
```

`playwright-cli --help` lista tudo o que a CLI pode fazer (`open`, `goto`, `snapshot`, `click`, `fill`, etc.).

### 3. Instalar as skills

Agentes de codificação como Claude Code e GitHub Copilot podem usar habilidades instaladas localmente para obter um contexto mais rico sobre os comandos disponíveis:

```bash
playwright-cli install --skills
```

### 4. Usar com o agente

Com tudo configurado, peça ao agente, por exemplo:

1. Navegue pelo https://www.saucedemo.com/ usando o Playwright CLI.
2. Explore o site e identifique o fluxo de trabalho do usuário mais crítico e, em seguida, gere um teste.

O agente usa a CLI para abrir o browser, capturar snapshots, interagir pelas refs (`e11`, `e15`…) e, a partir do fluxo explorado, gerar o teste Playwright no projeto.
