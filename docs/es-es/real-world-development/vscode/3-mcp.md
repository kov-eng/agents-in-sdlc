---
title: "Ejercicio 3: Probar la característica con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de cambios, conviene comprobar que funciona en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que pruebe la funcionalidad en un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Mediante MCP, los agentes pueden acceder a información actualizada (a través de recursos) y realizar acciones en tu nombre (mediante herramientas).

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP administra la comunicación entre el agente de IA y las herramientas externas, como las API existentes o las herramientas locales, por ejemplo, paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente de IA.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para administrar repositorios de GitHub. Permite al agente de IA realizar acciones como crear repositorios, actualizar los existentes y administrar incidencias y solicitudes de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. Permite al agente de IA realizar acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar un servidor MCP, revisa detenidamente su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Verás que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` forma parte de la plantilla del proyecto. Por eso Copilot pudo leer la lista de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La marca `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para sus pruebas de extremo a extremo, por lo que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita sus herramientas, VS Code inicia el servidor y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y autorízalo para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará (y te pedirá que lo autorices) la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite que Copilot controle un navegador real. En vez de recorrer la aplicación tú mismo para comprobar el trabajo, el agente puede abrir una página, navegar, aplicar filtros y comunicarte los resultados; después resumirá lo que ha observado. Es la forma más rápida de comprobar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright usa el [árbol de accesibilidad de la página][playwright-mcp-server] en lugar de capturas de pantalla. Así, el agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como hacen las tecnologías de asistencia. De esta forma, una comprobación funcional rápida también sirve como una verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad que acabas de crear:

```text
Usa el servidor MCP de Playwright para abrir un navegador en la aplicación en ejecución en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Borra el filtro y, después, aplica un filtro de editorial y confirma que la lista solo muestra los juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados cumplen ambos criterios.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que haya encontrado. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para hacer esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la solicitud. La primera vez que Copilot use el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de cambios

Ahora que has comprobado que la funcionalidad funciona, puedes abrir una solicitud de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, en github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, dejaremos que cree la PR. Puede encontrar la incidencia correspondiente y asociarla a la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en la lista desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una nueva solicitud de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que use una herramienta de GitHub, es posible que tengas que autenticarte. Sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud de cambios, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de cambios para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una característica, pero MCP es solo una forma de ampliar Copilot. A continuación, vamos a [crear un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
