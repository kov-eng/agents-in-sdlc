---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise juntos os filtros, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação dos filtros, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes foram salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez merge manualmente do pull request (PR) de avaliação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o trabalho de filtragem e qualidade em uma única conversa e branch, e incluiremos tudo neste PR.

Nesta lição, você vai:

- conhecer o Agent Merge e saber como ele automatiza o ciclo de vida do merge;
- inspecionar as alterações completas do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi integrado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo o controle da autorização de merge com o desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar um pull request. Quando você o habilita, o Copilot resolve os bloqueios do PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub, se o repositório permitir.

Até aqui, você mesmo selecionou **Merge pull request**. Agora, o Agent Merge pode cuidar dessa etapa. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que você recebeu uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório ou permissões ausentes. Se o processo for bloqueado, leia o motivo informado e faça o merge manualmente, depois da revisão, quando o repositório permitir.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação do código e, agora, o processo do pull request. Você:

- conheceu o Agent Merge e aprendeu como ele automatiza o ciclo de vida do merge;
- inspecionou as alterações completas do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Na próxima lição, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
