---
title: "Lección 4: Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una manera determinada, asegúrate de que Copilot disponga de esas indicaciones. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Después de crear el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizarás el archivo de instrucciones para garantizar que se sigan los estándares de programación;
- verás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su finalidad, parámetros y valores devueltos; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben actualizarse cuando cambie el código relacionado.
- Deben conservarse las pautas de formato y lint existentes.

Con los archivos de instrucciones proporcionarás a Copilot la información necesaria para que trabaje conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas dan a Copilot contexto y preferencias para que entienda mejor tu estilo y requisitos. Puedes especificar convenciones, bibliotecas y comentarios, y crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio y contiene información general pertinente para la mayoría de las tareas.
- `.github/instructions/*.instructions.md` contiene pautas para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Examínalos antes de hacer cambios para observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Examina la descripción del proyecto y las pautas de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de `.github/instructions`, donde hay instrucciones para Astro, la capa de datos Drizzle, pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del principio, que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y del proyecto. Así puedes dividir las instrucciones extensas en archivos pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir.

## Actualiza las instrucciones del equipo

Los archivos actuales son un buen punto de partida, pero falta una pauta. Modifica `copilot-instructions.md` para pedir comentarios TSDoc en el nuevo código TypeScript.

1. Busca la sección **Code formatting guidance** de `.github/copilot-instructions.md`, cerca de la línea 35.
2. Añade esta viñeta al final:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar los cambios, sal de la conversación y reanúdala para que Copilot utilice las nuevas pautas sin perder el contexto de la función.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la función. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las específicas de rutas;
- actualizaste el archivo de instrucciones para que se siguieran los estándares;
- observaste su efecto en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
