---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una manera determinada o Copilot necesita información previa, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las pautas de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas;
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influye el archivo de instrucciones cuando revises los pull requests que genere el agente en este recorrido.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto útil para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` sirven para tareas o tipos de archivos concretos. Puedes usarlos para lenguajes como TypeScript o Astro, o para tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen metadatos `applyTo`.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md.

[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

### Prácticas recomendadas para administrar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de este taller, pero el proyecto de ejemplo muestra un enfoque representativo. En general:

- Mantén `.github/copilot-instructions.md` centrado en la orientación general del proyecto: qué se está creando, cómo está organizado y qué estándares de programación se aplican.
- Usa `*.instructions.md` para pautas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural y claro. Incluye ejemplos de código correcto e incorrecto.

No existe una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar la mejor opción para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería contar con una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá encuentres instrucciones para muchas tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente al entorno indicará cuándo utilizarla.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos que incluye este repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas pautas se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: contiene un patrón glob relativo a la raíz del repositorio que determina los archivos aplicables. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, `**/*.test.ts`.
5. Fíjate en las instrucciones para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de comentario.

## Añade un nuevo estándar del repositorio

Ahora añadirás una regla general: la documentación debe incluir comentarios TSDoc en el código y un encabezado de comentario a nivel de archivo. El agente en la nube utilizará esta regla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. En el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha explicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye normas sobre documentación en el código.
3. Añade estas líneas de Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el repositorio (`.github/copilot-instructions.md`) para la orientación global o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirma, publica y fusiona la actualización

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, Copilot partirá de `main`, así que los cambios deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre un pull request de `custom-instructions-cloud` a `main` y fusiónalo. El agente en la nube leerá estas instrucciones al trabajar en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. El flujo con ramas permite practicar un proceso similar al de un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en los pull requests que genere el agente en la nube durante el resto del recorrido.

A continuación, [asigna tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
