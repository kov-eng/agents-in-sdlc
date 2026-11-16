---
title: "Lección 7: Crea y utiliza un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo un agente personalizado combina instrucciones, habilidades y herramientas MCP;
- crearás y examinarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir un pull request (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot, definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador, y el perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de los resultados. El perfil no reemplaza estas herramientas ni requiere iniciar otra conversación.

## Crea el perfil de QA

Antes de abrir el PR de la función, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Examina el perfil

Antes de utilizar el agente, comprueba que Copilot haya recogido el proceso y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio modifique la función cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos: `description` es obligatorio; `name` es opcional, pero ayuda a identificar el agente.
3. Revisa las instrucciones y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y utiliza Playwright MCP.
4. Confirma que el perfil presenta evidencias, pregunta antes de modificar el código de implementación y no confirma cambios ni abre pull requests.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado después de crear el perfil y selecciona QA para que pueda usar la incidencia y las decisiones de planificación.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que aún no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Pide al agente que revise la función:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Deja que el agente de QA trabaje.
4. Cuando termine, lee el informe.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- exploraste cómo un agente personalizado combina instrucciones, habilidades y herramientas MCP;
- creaste y examinaste un perfil reutilizable de QA;
- seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisión. A continuación, [reunirás todo en un PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
