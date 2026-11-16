---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información de contexto, conviene asegurarse de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que sirven para proporcionar directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones cuyo ámbito se limita a determinadas rutas;
- generarás el primer fragmento de datos para el filtrado (un helper de editoriales) con las instrucciones actuales;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volverás a enviar la misma solicitud y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás los cambios en las instrucciones y el fragmento de filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que establezcas. Copilot no es determinista. El objetivo de este ejercicio es observar la tendencia en el comportamiento tras actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Mediante archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con las prácticas indicadas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te ayuda a orientar las sugerencias para que sean más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y hasta los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, pertinente para la mayoría de las solicitudes de chat o CLI enviadas a Copilot. Puede incluir la pila tecnológica, una descripción general de lo que se está creando, buenas prácticas y directrices globales.
- Se pueden crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivos concretos. Por ejemplo, sirven para establecer directrices para lenguajes específicos, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan un campo `applyTo` en el frontmatter.
>
> Copilot también [admite archivos de instrucciones que usan otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para administrar archivos de instrucciones

Hablar en profundidad sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto de muestra muestran un enfoque representativo. A grandes rasgos:

- Mantén el archivo `copilot-instructions.md` centrado en directrices del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para directrices específicas de tipos de archivo (pruebas unitarias, componentes de Astro o la capa de datos) o tareas concretas.
- Usa lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería y no debería ser el código.

No existe una única forma de crear archivos de instrucciones, del mismo modo que no hay una única manera de usar la IA. La experimentación te permitirá descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá observes que hay instrucciones para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando explicará esta función cuando proceda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas directrices se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript (por ejemplo, uno que coincida con `**/*.test.ts`).
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos, añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crear una rama para los cambios

Sigamos algunas buenas prácticas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuáles debes usar.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una solicitud con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma solicitud para comparar los resultados.

1. Cierra las pestañas abiertas del editor de ejercicios anteriores para que Copilot solo use el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta solicitud:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en los archivos.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones de código, normalmente en el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto se debe a las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot funciona de forma probabilística, es posible que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de la *coherencia* tras actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como se ha indicado antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Vamos a documentar los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Ahí se documentan los estándares de programación del proyecto, pero todavía no hay ninguna regla sobre documentación en el código. Por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentarios de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a enviar la solicitud y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma solicitud** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique al código real sobre el que se basará el siguiente ejercicio.

## Confirmar las instrucciones y subir la rama

Los archivos de instrucciones son recursos del repositorio y se administran mediante el mismo sistema de control de versiones que los demás archivos. Confirmemos los cambios y subamos la rama al repositorio.

1. Abre una nueva ventana del terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, confirma que están presentes las actualizaciones de las instrucciones y del helper ejecutando:

   ```bash
   git status
   ```

3. En el terminal, prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya cambiado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de cambios

Una vez subida la rama, crearemos una solicitud de cambios (PR) y la vincularemos a la incidencia de los estándares de documentación de la lista de tareas. Podríamos hacerlo manualmente, pero Copilot puede hacerlo por nosotros con las herramientas de GitHub ya conectadas al proyecto. Le pediremos que encuentre la incidencia, cree una PR para cerrarla y la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub por ti. La plantilla del proyecto ya configura las herramientas de GitHub en tu espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot use una de ellas, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las actualizaciones de las instrucciones como la nueva base del helper de editoriales:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te solicite permiso para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la PR, pide a Copilot que la combine y vuelva a situar tu rama en `main` con esta solicitud:

   ```
   Combina la PR en main. Después, vuelve a main localmente y descarga el código más reciente para que todo esté actualizado.
   ```

7. Cuando Copilot te solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de cambios con la ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar una solicitud de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar la misma solicitud y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda continuar el trabajo.

En el [modo de agente][next-lesson], usarás esas instrucciones para que Copilot añada una nueva característica en todo el código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
