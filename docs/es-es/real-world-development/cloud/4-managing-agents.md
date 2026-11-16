---
title: "Ejercicio 4: Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas para mejorar la experiencia de usuario y añadir funciones. Aunque trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Puedes administrar las tareas asignadas al agente mediante varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes consultar todas las tareas con pull requests abiertos, revisar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas;
- dirigirás una sesión en curso para solicitar una función adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se está trabajando en los estilos y en la funcionalidad de alternancia, resulta lógico incluirlo. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente en la nube de Copilot:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas, tanto en el panel principal como en el izquierdo. Deberías encontrar las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot, como:
   - obtener el código del repositorio;
   - crear el entorno de trabajo;
   - configurar servidores MCP;
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Fíjate también en el panel del pull request que aparece a la derecha. Desde allí puedes consultar el PR y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre elegido por Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes similares a los que se muestran a continuación.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro para dirigir la sesión mientras Copilot trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud después de terminar el paso que estaba realizando cuando la enviaste.

El proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso para ajustar su alcance o dar indicaciones adicionales.

Has aprendido:

- a utilizar Mission Control y la página de agentes para supervisar tareas;
- a redirigir una sesión en curso para solicitar otra función.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
