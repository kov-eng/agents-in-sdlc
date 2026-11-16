---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos que les ayuden a realizar ciertos tipos de trabajo. Los equipos pueden convertir su experiencia en agentes reutilizables: uno de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene metadatos YAML con al menos `name` y `description`, seguidos de un prompt en Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar tareas. La forma más sencilla de distinguirlos es que un **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio conforme a las pautas de accesibilidad; para hacerlo, podría recurrir, por ejemplo, a una habilidad para gestionar pull requests o a otra para ejecutar y administrar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las adecuadas para tus entornos y casos de uso.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio explorarás:

- cómo se definen los agentes personalizados;
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que el tema oscuro resulta difícil de leer por el contraste insuficiente entre el texto y el fondo. Para resolverlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Para ello, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al codespace y examina el archivo del agente:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de los metadatos YAML.

> [!CAUTION]
> Los campos `name` y `description` de los metadatos son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Crea y asigna una incidencia

Mission control es el centro desde el que puedes trabajar con los agentes de tu entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas, redirigirlas y proporcionar indicaciones adicionales. Empecemos asignándole la creación del modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña de incidencias.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación al agente en la nube, con las opciones de agente personalizado y accesibilidad resaltadas](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes aprender a [supervisar y dirigir la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Preparación para usar agentes personalizados en tu organización][org-custom-agents]
- [Preparación para usar agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisión y administración de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
