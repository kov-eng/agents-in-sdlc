---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar filtros a partir de um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. No entanto, alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho existente da organização e ajudar você a criar os recursos certos da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes, começando com um issue para gerar um recurso, verificando se o código é válido e se o recurso funciona como esperado e, por fim, fazendo merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e a mesma branch enquanto continua o fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivo, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Para começar, nesta lição você vai:

- iniciar uma conversa do Copilot a partir de um issue do GitHub;
- definir requisitos no modo Plan;
- implementar o recurso com o modo Autopilot;
- revisar o código;
- validar o recurso manualmente em um navegador com porta encaminhada.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e distribuidora. O issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser acordados antes de escrever o código. Você usará o modo Plan para decidir esses detalhes e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar com um issue registrado que descreve o que precisa ser feito.
2. Criar um plano do que precisa ser desenvolvido.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Comece pelo issue

Antes de começar a criar os filtros, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os issues.
8. Use as teclas de seta para destacar o issue intitulado **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto um issue ou pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem de implementação de um recurso ou de execução de uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é sempre bom reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador de modo abaixo do prompt exibe **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano. Primeiro, ele explorará o projeto e depois determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano para revisá-lo.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementar o recurso. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que ele está resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de fazer merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo está correto.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação dos filtros e os testes.
3. Compare o resultado com o issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise o resultado das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros de categoria e de distribuidora, além das combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde ao issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um issue do GitHub;
- definiu requisitos no modo Plan;
- implementou o recurso no modo Autopilot;
- revisou o código;
- validou o recurso manualmente em um navegador com porta encaminhada.

Agora, vamos explorar melhor como gerar código que siga práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
