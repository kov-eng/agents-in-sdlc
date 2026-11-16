---
title: "Lección 9: Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra que permiten interactuar con ella y acceder a funciones avanzadas, información interna y opciones de configuración. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo administra Copilot el contexto de la conversación;
- usarás `/model` para examinar los modelos disponibles;
- aprenderás a exportar o compartir una sesión con `/share`;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora descubrirás algunas funciones adicionales: administrar el contexto, cambiar de modelo, compartir sesiones y delegar opcionalmente en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

En las tareas grandes o complejas puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes examinarla o compactarla con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` otra vez y compara el resultado. Si la conversación ya es pequeña, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas ventajas y cada persona tiene sus preferencias. Copilot CLI permite enumerar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Examina los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir un pull request.

Estos comandos son opcionales porque pueden crear árboles de trabajo o tareas remotas adicionales. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa el resultado con tu flujo habitual. Para profundizar en el trabajo asíncrono, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver qué ocurre en segundo plano. En esta lección:

- usaste `/context` y `/compact` para explorar la administración del contexto;
- usaste `/model` para examinar los modelos disponibles;
- aprendiste a exportar una sesión con `/share`;
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

Hay muchos más comandos de barra y funciones de Copilot CLI por explorar. Para terminar el recorrido, [repasemos lo aprendido][next-lesson] y los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Administración del contexto en Copilot CLI][context-management]
- [Información sobre el agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
