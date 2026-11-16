---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

La mayoría de las organizaciones tiene problemas de deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funciones que llevan tiempo en la lista de tareas pendientes. El agente en la nube de GitHub Copilot puede actualizar código y añadir funciones de forma autónoma. Al terminar, crea un PR en borrador para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. La página de detalles de cada juego debería mostrar otros títulos de la misma categoría para que los visitantes sigan explorando. Por ahora no necesitan una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia. Es un impedimento, aunque hay incidencias más prioritarias.

Son tareas que pueden quedar relegadas y que encajan bien con el agente en la nube. Este puede ocuparse de ellas de forma asíncrona mientras tú trabajas en otras cosas; después revisarás su resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma similar a una persona desarrolladora. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras la asignación, Copilot crea un pull request en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o cuando haya terminado. Cuando la solución esté lista para revisión, te mencionará en el pull request.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un asistente de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente, examina sin modificar los archivos de instrucciones del repositorio para que después puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes a Copilot la incidencia `Code lacks documentation`, observa si el PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas pautas proceden de los archivos de instrucciones; volveremos a revisarlas en un ejercicio posterior.

## Configura el entorno de desarrollo del agente

Crear código suele requerir un entorno específico y scripts de configuración. Lo mismo se aplica a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo y proporciona las herramientas y dependencias necesarias. El laboratorio ya incluye esta configuración: proporciona Node.js, dependencias, binarios de navegador para pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

Como en cualquier otro flujo de GitHub, hay algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

La mayoría de los proyectos tienen información desactualizada o carecen de documentación. Esta deuda técnica reduce la productividad y dificulta el mantenimiento o la incorporación de nuevas personas al equipo. Copilot puede ayudar a crear documentación; así que esta es una buena incidencia para asignarle.

1. En una nueva pestaña, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Code lacks documentation` en **Title**.
5. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot** y, a continuación, **Assign**.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

8. Selecciona la pestaña **Pull Requests** y abre el PR que acaba de generarse, con un título similar a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y vuelve a cargar la página.
9. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y decidiendo cómo resolver la tarea.

10. Revisa las tareas previstas. En la cronología del PR, comprueba que Copilot ha empezado a trabajar y selecciona **View session**.

  ![Vista de sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la página para ver el indicador actualizado.

11. Observa la sesión en directo: Copilot explora el código y el estado del proyecto, se detiene a planificar el enfoque y después crea código.

El proceso tardará unos minutos. El agente en la nube puede trabajar de forma asíncrona, de modo que puedes ocuparte de otras tareas. Aprovecharemos esa posibilidad para asignarle otra tarea mientras añadimos funciones a la aplicación.

## Añade una sección de juegos relacionados

Una ventaja del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Aunque añadir una sección de juegos relacionados no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. Asignemos esta tarea al agente.

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Issues**, después **New issue** y **Blank issue**.
3. Escribe `Show related games on the game details page` en **Title**.
4. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

5. Selecciona **Create**. A la derecha, selecciona **Assign to Copilot** y, a continuación, **Assign**.

Poco después aparecerá el emoji 👀 en el primer comentario de la incidencia: indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya se ocupa de la segunda solicitud. No es necesario supervisarlo continuamente; cuando termine, podrás revisar el resultado. Mientras tanto, aprenderemos a crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona, por ejemplo, para resolver deuda técnica, crear funciones o migrar código entre frameworks.

Has aprendido:

- a personalizar el entorno para generar código;
- a garantizar que las operaciones se realicen de forma segura;
- a definir incidencias con un alcance claro;
- a asignar incidencias a Copilot.

Mientras el agente trabaja, puedes aprender a crear y utilizar agentes personalizados. [El agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Información sobre el agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
