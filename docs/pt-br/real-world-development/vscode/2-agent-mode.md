---
title: "Exercício 2 - Adicionar funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações como executar testes. Como desenvolvedor, seu fluxo de trabalho costuma envolver localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... E por aí vai.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante à de um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como você pode usar o modo agente para adicionar novas funcionalidades ao seu site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente na nuvem foram criados para experiências diferentes. O modo agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem funciona como um programador parceiro, trabalhando de forma assíncrona como integrante da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- o modo agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados;
- revisar as alterações e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um auxiliar para editoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza suas páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um `<script>` Astro pequeno e com escopo limitado, usando as APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Você pode executá-la no GitHub Codespace seguindo estas etapas:

1. Volte ao Codespace. Você deve estar na branch `main` após o exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova branch para este trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com a URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo for aberta solicitando que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um Codespace, selecionar o link da URL localhost no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu Codespace, que agora está hospedando o servidor Web.

### Explorar o site

Com o site em execução, você pode explorar os recursos disponíveis. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam em cada exercício — cada etapa indicará essas opções quando forem relevantes.

A implementação inicial do site está funcional, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot para exibi-las.

1. Selecione **Agent** no menu suspenso de agentes da exibição Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo em particular. Ao criar este workshop, usamos o Claude Sonnet 4.5 e, por isso, o incluímos nas instruções. A intenção é que as sugestões de código recebidas sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, talvez as sugestões recebidas sejam diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot informações sobre o backlog de issues enviando o seguinte prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, é uma boa hora para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém informações sobre como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, que incluem:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar conteúdo, dividir conjuntos complexos de instruções em arquivos menores e mais fáceis de gerenciar, além de fornecer exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar os testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O modo agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Você pode continuar a conversa atual com o Copilot ou selecionar **New Chat** para iniciar uma nova.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no menu suspenso de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog usando o seguinte prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot começar explorando o projeto e localizando os arquivos relacionados à funcionalidade desejada. Você verá que ele encontra os auxiliares da camada de dados, a interface e os testes. Em seguida, começa a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá o Copilot realizar várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez os botões **Keep** e **Undo** apareçam no editor de código. Quando o Copilot terminar, você verá **Keep** ou **Undo** para todas as alterações, então não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel Copilot Chat solicitando a confirmação de que o usuário autoriza a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e até falhas em alguns testes durante o processo. Não tem problema! O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat com o modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador onde o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados;
- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar seu recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício de exploração bônus – Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use o que aprendeu neste exercício para pedir ao Copilot que atualize o site e implemente a paginação. Considere os seguintes aspectos:

- siga as práticas recomendadas existentes, inclusive o uso dos arquivos de instruções disponíveis;
- decida como implementar a paginação: se o usuário poderá selecionar o tamanho da página ou se ele será definido no código;
- ao criar o prompt, forneça ao Copilot as orientações necessárias para que a implementação atenda às suas expectativas;
- talvez seja necessário iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
