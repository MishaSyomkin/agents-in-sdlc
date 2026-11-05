---
title: "Ejercicio 2: Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos en una aplicación suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. El flujo habitual de desarrollo puede implicar localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, encontrar el archivo que faltaba y volver a modificarlo. El modo de agente de Copilot puede coordinar estos pasos.

El modo de agente puede explorar la estructura del proyecto, actualizar los archivos necesarios, ejecutar pruebas y corregir los errores que detecte. En este ejercicio lo usarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE para ofrecer ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona como un compañero de equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot explora el proyecto, identifica archivos y coordina cambios;
- el modo de agente implementa funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de fusionarlos.

## Escenario

A medida que crece el listado de juegos, quieres que se puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade con un pequeño `<script>` de Astro que usa las API estándar del DOM.

### Inicia el sitio

El proyecto incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtros:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost del terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, revisa sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo pueden variar en cada ejercicio; aquí se indican cuando corresponde.

La implementación inicial funciona, pero queremos añadir capacidades. Empieza por revisar el backlog de incidencias que se creó automáticamente a partir de la plantilla.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El modo de agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y usar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Durante la creación del taller se usó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero los resultados pueden variar. Es normal, ya que los modelos de lenguaje grandes (LLM) son probabilísticos.

> [!NOTE]
> Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según el valor que considere que tienen para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que aporta contexto para esta tarea. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices, que incluyen:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, pruebas y accesibilidad;
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace. Así se pueden reutilizar instrucciones, dividir conjuntos complejos y proporcionar ejemplos y plantillas a Copilot.

## Implementa los filtros

Para completar los filtros hacen falta, como mínimo, tres cambios:

- añadir o mejorar la lógica restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Las pruebas deben terminar correctamente antes de fusionar los cambios. El modo de agente puede coordinar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Seleccionar el archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría según los requisitos de la incidencia relacionada de GitHub. Ya existe un helper de editoriales del ejercicio anterior; consérvalo y mejóralo si es necesario mientras completas el trabajo restante en la capa de datos, la interfaz y las pruebas. Comprueba que todas las pruebas pasan antes de terminar. El servidor ya está en ejecución; no hace falta volver a iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos pertinentes en la capa de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, modificar archivos y ejecutar pruebas. Mientras trabaja, el editor puede mostrar los controles **Keep** y **Undo**. Cuando termine, aparecerá una opción para aceptar o deshacer cada cambio; no hace falta seleccionarla mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot solicita confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. Copilot alternará entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.
9. Explora el código generado para detectar posibles problemas.

   ![Sesión completada con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y prueba la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has usado el modo de agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido cómo:

- el modo de agente puede implementar funcionalidades en la interfaz y la capa de datos;
- el modo de agente puede explorar el proyecto, identificar archivos y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de fusionarlos.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca el listado de juegos, puede que necesites paginación. Aplica lo aprendido y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas existentes y usa los archivos de instrucciones;
- decide cómo quieres implementar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página;
- incluye en el prompt las pautas necesarias para obtener la implementación que necesitas;
- puede que tengas que iterar con Copilot, solicitar cambios y aportar contexto adicional.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
