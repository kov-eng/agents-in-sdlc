---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que documentara el código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar e implementara mejoras de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya hemos comentado, la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual. Revisaremos las sugerencias de Copilot para la documentación, los juegos relacionados y la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el [acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por el primer pull request (PR) que generó el agente en la nube: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que el PR muestre un aviso de que Copilot está bloqueado por un firewall. Es **lo esperado**: de forma predeterminada, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre el pull request con un título parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título del pull request incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando el PR esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluirá comentarios TSDoc nuevos y otra documentación. Los cambios concretos pueden variar. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario: proceden de los archivos de instrucciones que examinaste al comienzo del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones del PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios del PR o en comentarios en línea del código, como harías con otra persona del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una única instrucción válida. Por ejemplo, puedes pedirle que:

- añada al principio de cada archivo de código un encabezado con una breve descripción;
- añada comentarios TSDoc a los archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**. Puedes usar una de las ideas anteriores o pedir otra mejora de documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver al PR.

    ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en el PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar el PR como listo para revisión y fusionarlo en la rama principal.

![Convertir el PR en listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos al PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR con un título parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones del PR. Si todo va bien, deberían superarse las comprobaciones del proyecto para la aplicación Astro. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la función. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando y pedirle que ajuste, por ejemplo, cuántos juegos relacionados se muestran o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade un comentario en la pestaña **Conversation** y Copilot iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones implementadas con el agente personalizado de accesibilidad. Este PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado en Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR con un título parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, el pull request incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación, prestando atención a:
   - los componentes de alternancia de la interfaz para cambiar de modo;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios CSS o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones. Si todo va bien, deberían superarse las comprobaciones del proyecto para la aplicación Astro. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en Codespaces para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para que implemente estas funciones siguiendo las prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, puedes solicitar cambios mencionando a **@copilot** en un comentario, como hiciste con los otros PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegarle tareas reales de la lista de trabajo mientras te ocupas de otras cosas. Para practicar, crea más incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés en un juego en la página de detalles;
- implementar la paginación de la lista de juegos;
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Examinaste los archivos de instrucciones personalizadas** del repositorio para ver su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre los pull requests**, mencionaste a `@copilot` para solicitar cambios y aprobaste los flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los demás recorridos complementan lo que has aprendido:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

Prueba también estas ideas en tu propio repositorio:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
