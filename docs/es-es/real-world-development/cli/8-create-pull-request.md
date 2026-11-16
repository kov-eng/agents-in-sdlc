---
title: "Lección 8: Crea y fusiona el PR de la función"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después crea un PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardadas en una misma rama. Ahora las revisarás en conjunto y abrirás un pull request. Fusionaste manualmente el PR anterior de las valoraciones; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la función, las actualizaciones de instrucciones, la habilidad y el agente de QA en varios PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de fusión;
- examinarás todos los cambios de la función y las evidencias de verificación;
- crearás el PR del filtrado;
- activarás Agent Merge solo después de revisarlo y confirmarás la fusión.

## Escenario

Has utilizado Copilot para planificar, implementar y verificar una función. Tailspin Toys quiere automatizar el trabajo restante del PR y mantener el control de la autorización para fusionarlo.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar un pull request. Al activarlo, Copilot resuelve los impedimentos del PR —por ejemplo, corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta— y, después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de ello, pero debes revisar el trabajo y confirmar que está listo antes de activarlo.

## Deja que Agent Merge gestione el PR

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de agentes.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea un PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua del PR y lo fusionará cuando todo se haya completado correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo, y gestionar el pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de fusión;
- examinaste todos los cambios de la función y las evidencias de verificación;
- creaste el PR de filtrado;
- activaste Agent Merge después de la revisión y confirmaste que el PR se fusionó.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Administrar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
