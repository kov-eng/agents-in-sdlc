---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se é que alguma — não enfrentem problemas com dívida técnica. Pode ser a existência de problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um rascunho de PR para revisão de um desenvolvedor. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Você explorará os seguintes recursos do agente de nuvem do Copilot:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de delimitar bem os issues;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas questões de dívida técnica que gostaria de resolver. Os prestadores de serviço inicialmente contratados para criar a primeira versão do site deixaram a documentação em um estado insatisfatório — na verdade, você verá que ela está completamente ausente. Como primeira etapa, a equipe gostaria de adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Por enquanto, não é necessária uma implementação refinada; basta algo que possa ser usado para testes de aceitação da experiência do usuário. Essa tarefa está bloqueada, mas há outras questões de prioridade mais alta no momento.

Esses são dois exemplos de tarefas que podem ser despriorizadas rapidamente e são ótimas candidatas para atribuir ao agente de nuvem do Copilot. O agente poderá trabalhar nelas de forma assíncrona, enquanto você se dedica a outras tarefas. Depois, você poderá voltar para revisar o trabalho e confirmar que tudo está como esperado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a um desenvolvedor. Assim como no trabalho com um desenvolvedor, há várias maneiras de usá-lo, incluindo [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que recebe a atribuição, o Copilot cria um rascunho de pull request para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode examinar a sessão do Copilot enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, ele marcará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: não dá para deixar de pensar. Até operações aparentemente simples ficam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir o issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções que você acabou de examinar. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código, independentemente de quem esteja envolvido, costuma exigir um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de trabalho especial de configuração][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse fluxo já foi configurado para facilitar o andamento do laboratório e permitir este aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as menores permissões necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e popule o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona, em vez de esperar o Copilot executá-lo.

## Adicione documentação

Todos entendem a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma não ser resolvido, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores à equipe. Felizmente, o Copilot é muito eficiente na criação de documentação, o que torna essa uma tarefa ideal para o agente de nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar um issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão realizadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar no issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot está resolvendo o problema. Isso inclui explorar o código e entender o estado do projeto, pausar para pensar e decidir a abordagem adequada e criar código.

Esse processo provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é permitir que ele execute tarefas de forma assíncrona, liberando você para cuidar de outras atividades. Vamos aproveitar esse recurso: enquanto atribuímos outra tarefa ao agente, voltaremos nossa atenção para escrever código e adicionar recursos ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele cuida de outro. Mesmo que a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, esse tempo poderia ser usado em outras atividades. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário do issue, indicando que o Copilot está trabalhando nele.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na sua segunda solicitação. O agente de nuvem trabalha de forma semelhante a um engenheiro de software. Portanto, você não precisa acompanhá-lo ativamente: basta revisar o resultado quando estiver pronto. Agora vamos nos concentrar em criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você conheceu o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. Use o Copilot para lidar com dívida técnica, criar recursos ou ajudar na migração de código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de delimitar bem os issues;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, podemos nos concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
