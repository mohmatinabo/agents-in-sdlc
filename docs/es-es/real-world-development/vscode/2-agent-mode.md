---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen implicar cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de desarrollo puede incluir localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otro cambio… y así sucesivamente.

El modo agente de Copilot puede ayudarte con este trabajo.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentre. Veamos cómo usarlo para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite recibir comentarios y colaborar rápidamente. El agente en la nube está diseñado para trabajar de forma asíncrona como un miembro del equipo e interactuar contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios entre ellos.
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editorial. En el ejercicio anterior ya creaste un asistente para editoriales. Ahora terminarás el trabajo pendiente en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de usuario en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha preparado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace así:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el comando siguiente para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL local en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuándo sea importante.

La implementación inicial del sitio funciona, pero queremos añadir nuevas funciones. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios de forma autónoma en varios archivos, ejecutar comandos de terminal e invocar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Al preparar el taller usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para que las sugerencias sean relativamente uniformes. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias que recibas sean distintas. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot el prompt siguiente para consultar el backlog de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado las incidencias según las que considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo abordar los cambios de funcionalidad del sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades. Incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivo, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios independientes:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar y superar las pruebas antes de combinar los cambios en el código base. El modo agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad: tanto los asistentes de la capa de datos y la interfaz de usuario como las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Durante el proceso, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá una opción **Keep** o **Undo** para cada cambio. No es necesario seleccionarlos mientras Copilot siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys! En este ejercicio has aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- el modo agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios entre ellos.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Al planificar el código:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- al crear el prompt, proporciona a Copilot las directrices necesarias para que implemente la solución que quieres.
- es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es el proceso habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
