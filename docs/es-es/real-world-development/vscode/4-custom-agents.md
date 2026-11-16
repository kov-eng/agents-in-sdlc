---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados, adaptados a tareas o ámbitos específicos del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones, buenas prácticas, patrones de programación y conocimientos específicos del ámbito para ayudarle a realizar ciertos tipos de trabajo de forma más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: un agente de accesibilidad que garantice el cumplimiento de las normas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos los campos `name` y `description`, seguido de una solicitud en Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Hay cierta coincidencia entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La forma más sencilla de distinguirlos es pensar que un **agente personalizado** es quien realiza el trabajo y las **habilidades** son sus herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Para ello, podría recurrir a habilidades, como una que gestione el flujo de trabajo de las solicitudes de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma «correcta» de crear un agente personalizado. Como con cualquier otro uso de la IA, prueba distintas opciones e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es fundamental, queremos implementar esta característica cuanto antes. Para generar la funcionalidad, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve al codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las buenas prácticas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Una lista de errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en la lista desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuáles debes usar.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat, si aún no está seleccionado.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista desplegable de agentes, situada en la parte inferior de la vista Chat (muestra el agente activo, que de forma predeterminada es **default**).
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía esta solicitud al agente de accesibilidad:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para activar o desactivar el alto contraste, y la configuración debe persistir después de recargar la página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat empezará a trabajar: explorará el código base, propondrá cambios y los aplicará a los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que podrás revisar.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar el conjunto de archivos que ha modificado el agente.

> [!NOTE]
> Es probable que el proceso tarde unos minutos. Copilot está realizando cambios reales en el repositorio: editará archivos existentes, como los componentes de Astro, CSS y las pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás el trabajo mientras se realiza.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados de GitHub Copilot para tareas y ámbitos específicos. Con ellos, puedes plasmar la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para realizar ciertos tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se realizan y añadirás un control para el tema claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la característica con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
