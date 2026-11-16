---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer — você precisa garantir que esse contexto esteja acessível. Há várias maneiras de compartilhar contexto específico com o Copilot. Entre as principais estão os [arquivos de instruções][instruction-files], usados para fornecer orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo definido por caminho;
- gerar a primeira parte dos dados para filtragem (um auxiliar para editoras) com as instruções atuais em vigor;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão;
- confirmar a atualização das instruções e a parte da funcionalidade de filtragem na branch `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após atualizar as instruções, não obter exatamente a mesma saída, caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa ter testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o que ele faz.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso pode ajudar você a direcionar o Copilot para sugestões e trechos de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas para o repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou à CLI. Isso pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas (como TypeScript ou Astro) ou para tarefas como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para conclusões de código nem sugestões de próximas edições.
>
> O Copilot Chat, a CLI do Copilot e o agente na nuvem do Copilot usam tanto os arquivos de instruções do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações sobre o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de codificação.
- Use arquivos `*.instructions.md` para fornecer orientações específicas para tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural. Escreva orientações claras. Inclua exemplos de como o código deve (e não deve) ser.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Você descobrirá, por meio de experimentação, o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá que há instruções para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` na CLI do Copilot) — a lição da interface que você está usando indicará quando isso for relevante.
>
> Quer encontrar modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal, `copilot-instructions.md`, e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Essas orientações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Aqui, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de indicar exemplos a seguir ao gerar código. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para nossas alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para nossas alterações.

1. Volte ao Codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam em cada exercício — cada etapa indicará essas opções quando forem relevantes.

Para ver o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para ver a diferença.

1. Feche as guias de editor abertas de exercícios anteriores para que o Copilot use apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o auxiliar deve ficar.
3. Selecione **Agent** no menu suspenso de agentes da exibição Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes abrangendo o arquivo auxiliar e seus testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicável a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, há uma chance de ele adicionar comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo é observar a melhoria de *consistência* após atualizar as instruções.

## Adicionar um novo padrão para o repositório

Como destacado anteriormente, `.github/copilot-instructions.md` foi criado para fornecer informações do projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de codificação do projeto — mas ainda não há nenhuma regra para a documentação no código, e é por isso que o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo após os padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões aplicáveis ao projeto inteiro, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico da conversa.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de direcionar o Copilot para seguir um novo padrão do projeto e aplicá-lo a código real que servirá de base para o próximo exercício.

## Confirmar as instruções e enviar a branch

Os arquivos de instruções são gerenciados pelo controle de versão da mesma forma que qualquer outro recurso do repositório. Vamos confirmar as alterações e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as atualizações de instruções e do auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. No terminal, prepare e confirme a atualização das instruções e todos os arquivos alterados pelo Copilot para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Com a branch enviada, vamos criar um pull request e vinculá-lo à issue sobre os padrões de documentação no backlog. Poderíamos fazer isso manualmente, mas o Copilot pode cuidar disso usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir ao Copilot para localizar a issue, criar um PR que a encerre e, em seguida, mesclar o PR.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao seu workspace, então não é necessário configurar nada — na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício futuro.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no seletor de agente.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a nova base do auxiliar para editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando solicitado a permitir que o Copilot execute ações no GitHub em seu nome, revise o comando e selecione **Allow**, se apropriado.
6. Depois que o PR for criado, peça ao Copilot que o mescle e retorne sua branch para `main` usando o seguinte prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir que o Copilot execute ações no GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow**, se apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot!

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o que o Copilot produz com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- confirmar as instruções e a base do auxiliar na branch `main` para que o próximo exercício possa usá-las.

Agora, você usará essas instruções no [modo agente][next-lesson] enquanto o Copilot adiciona um novo recurso ao código.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
