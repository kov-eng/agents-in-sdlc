---
title: "Lección 2: Añade valoraciones con estrellas"
description: "Haz un pequeño cambio en las tarjetas de juegos con Copilot CLI, revísalo en un navegador y fusiónalo en tu primer pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto: los juegos ya tienen una valoración con estrellas en sus datos, pero no se muestra en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás en tu primer pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad;
- pedirás al agente un cambio pequeño;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado;
- abrirás y fusionarás tu primer pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya se muestra en la página de detalles. Sin embargo, las tarjetas de juegos de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama que están abiertos en el terminal, en lugar de crear un árbol de trabajo específico. Las conversaciones guardadas permiten volver al mismo diálogo, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tus prompts y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes examinar con `/diff`.

## Inicia una conversación y solicita el cambio

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia la CLI con:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con `/new` y pulsa <kbd>Enter</kbd>.
5. Solicita el cambio con este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una función con Copilot CLI!

## Revisa el diff

Antes de fusionar, hay que revisar todos los cambios generados por IA, aunque sean pequeños.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones y pide a Copilot que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Copilot, como cualquier herramienta de IA generativa, es probabilístico, no determinista. El código exacto puede variar; revisa el comportamiento, no esperes una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y suponer que funciona. Pide a Copilot que inicie el sitio para que puedas inspeccionar la interfaz de usuario (UI) en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primer pull request

Ya has creado la función. Ahora abre un pull request (PR) para fusionar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear el PR y mostrará su URL.
3. Abre el PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando todo esté listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación centrada en una rama de funcionalidad;
- pediste al agente un cambio pequeño en las tarjetas;
- revisaste el cambio con `/diff`;
- ejecutaste la aplicación y comprobaste la valoración en un navegador reenviado;
- abriste y fusionaste tu primer pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
