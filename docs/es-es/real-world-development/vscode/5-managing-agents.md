---
title: "Ejercicio 5: Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con solicitudes de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones de archivos propuestas;
- orientarás al agente durante la sesión para añadir un control del tema claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha presentado otra solicitud: también quiere un control para el tema claro destinado a los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas de archivos, ediciones y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías recibir un mensaje final que resuma los cambios realizados.

## Orientar al agente para añadir el tema claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación para añadir un control del tema claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía esta solicitud de seguimiento en el cuadro de entrada de Copilot Chat:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control del tema claro junto al control de alto contraste. El tema claro debe seguir el mismo patrón: persistir en el almacenamiento local y estar disponible en la misma zona de la interfaz de usuario. El usuario debe poder activar el alto contraste y el tema claro de forma independiente.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá modificaciones para añadir el tema claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente sigue una dirección que no te convence —por ejemplo, si intenta reorganizar la interfaz de los controles de una forma que no te gusta—, puedes detenerlo con el botón **Stop**, situado junto al cuadro de entrada del chat, y enviar un mensaje de seguimiento para aclarar lo que quieres.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía solicitudes de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el tema claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios realizados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina los diffs. Comprueba que se apliquen los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones de archivos a medida que aparecían, las has aceptado o rechazado y has enviado solicitudes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real;
- aceptar y rechazar ediciones de archivos individuales;
- orientar una sesión en curso con solicitudes de seguimiento.

En la [siguiente lección][next-lesson], publicarás tu trabajo de accesibilidad como solicitud de cambios y revisarás las modificaciones que el agente personalizado local ha realizado durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
