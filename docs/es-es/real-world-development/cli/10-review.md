---
title: "Lección 10: Repaso y próximos pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo completo en Tailspin Toys. En este recorrido:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado;
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primer pull request (PR);
- partiste de la incidencia de filtrado, definiste el enfoque con Plan, lo implementaste con Autopilot y lo revisaste en modo Interactive;
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la utilizaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Playwright Model Context Protocol (MCP) y lo utilizaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las evidencias del navegador;
- revisaste el cambio completo y autorizaste Agent Merge para el PR de filtrado;
- exploraste comandos de barra para administrar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Qué has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones con estrellas**: muestra en las tarjetas de juegos `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la función, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR utilizaste la misma conversación y rama. Para simplificar el taller, reunimos todo ese trabajo en un único PR.

## Diferentes tipos de verificación

Comprobaste el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y dejando que Copilot lo explorara mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y de la cobertura de pruebas antes del PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA no requiere más pruebas, puede ser correcto. Revisa el código y las evidencias antes de autorizar la fusión, y actualiza las evidencias afectadas si se producen cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para acordar un enfoque antes de implementarlo, **Interactive** para seguir de cerca los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* quieres crear, *por qué* y *cómo* influye de manera significativa en el resultado.

## Más cosas por explorar

Ya has cubierto el flujo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Administrar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
