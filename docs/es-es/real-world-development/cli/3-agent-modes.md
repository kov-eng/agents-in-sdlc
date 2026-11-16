---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has añadido una función pequeña al proyecto. Para los cambios más amplios hace falta un proceso más completo. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización. En esta y las siguientes lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una función, comprobarás el código y el comportamiento, y finalmente lo fusionarás en el proyecto.

> [!NOTE]
> Durante todo el flujo de trabajo de la función utilizarás la misma conversación y rama. Normalmente se usarían distintas ramas o PR para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la función en el modo Autopilot;
- revisarás el código;
- validarás manualmente la función en un navegador reenviado.

En las siguientes etapas actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás el PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y las personas que visitan el sitio necesitan filtrar los juegos por categoría y editorial. La incidencia del trabajo pendiente describe la función, pero hay detalles —como la combinación de categorías— que deben acordarse antes de programar. Usarás Plan para resolver esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

Añadir agentes de programación con IA no cambia los fundamentos del desarrollo; de hecho, los hace aún más importantes. Un flujo habitual consiste en:

1. Partir de una incidencia que describa el trabajo.
2. Crear un plan de implementación.
3. Programar y revisar el código.
4. Ejecutar pruebas para validarlo.
5. Comprobar manualmente la nueva función.
6. Crear un pull request (PR).
7. Tras la revisión y la integración continua, fusionar el código.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: trabajas con el agente. Sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: primero crea un plan y no puede editar los archivos del proyecto.
- **Autopilot**: trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que la tarea está terminada.

Empieza en Plan, revisa el plan y después usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de iniciar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que el PR de las valoraciones con estrellas está fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para seleccionar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Verás que el prompt empieza por `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o un pull request de GitHub.

## Planifica la función de filtrado

La planificación permite acordar el enfoque antes de delegar la implementación. Es recomendable dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan:

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar a Plan. Confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que Copilot pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte que no refleje tus decisiones.

## Autoriza Autopilot

Con el plan acordado y revisado, es hora de implementarlo. Autopilot permite que Copilot itere hasta considerar que la tarea está terminada.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador del modo muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y verifica la implementación

Como cualquier otro código, el resultado debe revisarse antes de fusionarlo. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Tras revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diff.
5. Revisa las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Prueba la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría, de editorial y las combinaciones acordadas en el plan.
4. Confirma que el reinicio de los filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y próximos pasos

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección:

- iniciaste una conversación de Copilot a partir de una incidencia;
- definiste los requisitos en Plan;
- implementaste la función en Autopilot;
- revisaste el código;
- validaste la función manualmente en un navegador reenviado.

A continuación, [usarás instrucciones personalizadas][next-lesson] para que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
