---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados e adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento específico do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que garante a conformidade com as diretrizes [WCAG][wcag], um agente de segurança que segue práticas seguras de programação ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar operações. A maneira mais simples de diferenciá-los é: um **agente personalizado** é o executor; as **skills** são as ferramentas.

Os agentes personalizados têm uma janela de contexto própria e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante essa tarefa, ele poderá chamar skills, como uma skill para executar um fluxo de trabalho de pull request ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com os agentes personalizados, você explorará:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos, independentemente das capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar isso o mais rápido possível. Para gerar essa funcionalidade, usaremos um agente personalizado.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar seu conteúdo e entender como ele orientará o Copilot.

Volte ao seu codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine e revise as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - armadilhas e erros comuns.

## Crie e atribua um issue

O centro de controle é o local principal para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar tarefas e até redirecionar uma sessão em andamento ou fornecer orientações adicionais. Vamos começar atribuindo ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia de issues.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar um issue em branco.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem com o agente personalizado Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Nesta lição, você conheceu os [agentes personalizados][custom-agents] do GitHub Copilot: assistentes de IA especializados e adaptados a tarefas e domínios específicos. Com eles, você pode transformar a experiência e os padrões da sua equipe em agentes reutilizáveis para orientar o Copilot com mais eficiência.

Você explorou estes conceitos:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, podemos nos concentrar em [monitorar e direcionar a sessão do agente][next-lesson] pelo centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
