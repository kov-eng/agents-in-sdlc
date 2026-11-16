---
title: "Ejercicio 2: Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, volver a actualizarlo... Y la lista continúa.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, lo que permite ciclos de respuesta rápidos e interacción directa. El agente en la nube está pensado para trabajar como un compañero de programación asíncrono que interactúa contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto e identificar archivos pertinentes para realizar cambios coordinados;
- el modo de agente de GitHub Copilot puede implementar nuevas características en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper para las editoriales. Ahora completarás el resto del trabajo en la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y consultar sus detalles. El sitio web es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en el frontmatter, sin una API de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado que usa las API DOM estándar.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha proporcionado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespace siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` tras completar el ejercicio anterior.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Al seleccionar un enlace a la URL localhost en el terminal del codespace, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar sus características. Entre las principales se encuentran:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuáles debes usar.

La implementación inicial del sitio web funciona, pero queremos ampliarla con nuevas características. Empecemos por revisar la lista de tareas. Al configurar el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat. El modo **Agent** puede planificar e implementar cambios de forma autónoma en varios archivos, ejecutar comandos en el terminal e invocar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente consistentes y la experiencia sea satisfactoria. Sin embargo, como los LLM son probabilísticos, las sugerencias que recibas pueden diferir de las indicadas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre la lista de tareas enviándole esta solicitud:

   ```plaintext
   Muéstrame la lista de tareas del repositorio de GitHub. Ayúdame a priorizarlas según las que resulten más útiles para el usuario.
   ```
4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para el usuario.

## Revisar los archivos de instrucciones

Antes de poner al agente a generar código, conviene revisar el archivo de instrucciones que usarás para proporcionar contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene información sobre cómo añadir funcionalidad al sitio web.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios para el diseño de componentes, su capacidad de prueba y la accesibilidad.
   - Enlaces a archivos de instrucciones específicos para varios tipos de archivos, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar a Copilot ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres actualizaciones distintas en la aplicación:

- Añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista de tareas con esta solicitud:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia relacionada de GitHub en la lista de tareas. Ya existe un helper de editoriales del ejercicio anterior, así que consérvalo y perfecciónalo según sea necesario mientras completas el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar tanto los helpers de la capa de datos como la interfaz de usuario y las pruebas. A continuación, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Observarás que Copilot realiza varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Mientras tanto, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, verás las opciones **Keep** o **Undo** para todos los cambios, así que no hace falta seleccionarlas mientras el trabajo siga en curso.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio hemos visto cómo usar el modo de agente de GitHub Copilot para añadir características al sitio web Tailspin Toys. Hemos aprendido cómo:

- el modo de agente de GitHub Copilot puede implementar nuevas características en la interfaz de usuario y la capa de datos;
- el modo de agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y realizar cambios coordinados;
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

Ahora vamos a [probar la característica con el servidor MCP de Playwright][next-lesson] y a crear una solicitud de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que implemente la paginación en el sitio. Ten en cuenta lo siguiente:

- Sigue las buenas prácticas existentes, incluidos los archivos de instrucciones actuales.
- Piensa cómo quieres implementar la paginación: por ejemplo, si permitirás que el usuario seleccione el tamaño de página o si lo fijarás en el código.
- Al redactar la solicitud, proporciona a Copilot las directrices necesarias para que implemente la solución que quieres.
- Es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
