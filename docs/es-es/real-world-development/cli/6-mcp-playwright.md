---
title: "Lección 6: Valida la funcionalidad con Playwright MCP"
description: "Examina o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Para escribir código también hay que trabajar con datos y servicios externos, y ofrecer automatizaciones adicionales a Copilot. Para ello se utilizan los servidores MCP: permiten ampliar las herramientas y los servicios integrados en la CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio como lo haría una persona y automatizar las comprobaciones para aumentar la confianza en el resultado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada y realicen acciones en tu nombre.

Estas herramientas y recursos se conectan mediante un servidor MCP, que actúa como puente entre el agente y los servicios externos. Cada servidor ofrece un conjunto diferente de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para administrar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: permite automatizar el navegador con Playwright.

Hay muchos más servidores MCP. GitHub aloja un [registro MCP][mcp-registry] que facilita descubrirlos y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Copilot CLI ya tiene configurado Playwright MCP, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe este comando para abrir el cuadro de diálogo de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo.

## Pide a Copilot que explore la función con Playwright

Ya has comprobado manualmente el comportamiento. Ahora pídele a Copilot que haga lo mismo mediante el servidor Playwright MCP:

1. Envía este prompt:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible pedir explícitamente a Copilot que use el servidor MCP, porque suele detectarlo por sí solo. Sin embargo, indicárselo puede mejorar la coherencia de los resultados y reducir el número de tokens.

2. Observa los pasos que ejecuta en el navegador para comprobar que la función funciona.
3. Lee el informe y confirma que el comportamiento es el esperado.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te proporcionará un informe.

## Resumen y próximos pasos

¡Enhorabuena! Has utilizado Playwright MCP para explorar la función en un navegador real desde Copilot CLI. En esta lección:

- aprendiste qué es MCP y cómo lo usa Copilot CLI;
- añadiste Playwright MCP si no estaba configurado;
- pediste al agente que controlara un navegador y examinara el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
