---
title: "Ejercicio 3: Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Modo de agente][previous-lesson] · [Siguiente ejercicio: Agentes personalizados →][next-lesson]

Acabas de crear los filtros con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funcionan en el navegador. En vez de probarlos manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot use un navegador real. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente los filtros en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para los filtros.

## ¿Qué es MCP?

El modo de agente es más potente cuando puede acceder a herramientas fuera del editor. MCP permite que Copilot se comunique con herramientas y servicios externos mediante un protocolo estándar.

![Diagrama del funcionamiento del modo de agente y su interacción con el contexto, el modelo de lenguaje grande (LLM) y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente y gestiona la comunicación con herramientas externas, como interfaces de programación de aplicaciones (API) existentes o paquetes npm locales. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador mediante Playwright. El agente puede navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías encontrar un servidor `github` ya configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto incluye el servidor `github`; por eso, Copilot pudo leer el backlog en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hace falta instalarlo localmente. VS Code se conecta por HTTP y te pedirá que te autentiques con GitHub la primera vez que Copilot use sus herramientas.

## Añade el servidor Playwright MCP

Añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor `github` remoto, `playwright` es un servidor **local**. VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin una ventana visible, necesaria en Codespaces porque no hay un escritorio que mostrar.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP cuando se necesitan. La primera vez que Copilot requiera sus herramientas, VS Code iniciará el servidor y te pedirá que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en ella para iniciarlo.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confíes en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El servidor Playwright MCP permite a Copilot usar un navegador real. Puede abrir una página, navegar por ella, aplicar filtros, leer los resultados y resumirlos. Así puedes comprobar el comportamiento sin salir de la conversación.

Internamente, el servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados —botones, enlaces y elementos de lista—, como lo hace la tecnología de asistencia. Por eso, una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros.

1. Envía este prompt:

   ```text
   Usa el servidor Playwright MCP, abre un navegador en la aplicación que está en http://localhost:4321 y comprueba la nueva funcionalidad de filtrado de juegos:

   1. Ve a la página de juegos e indica cuántos aparecen.
   2. Aplica un filtro de categoría y confirma que el listado solo muestra juegos de esa categoría.
   3. Quita el filtro y aplica uno de editorial. Confirma que el listado se actualiza para mostrar esa editorial.
   4. Combina un filtro de categoría y otro de editorial y confirma que los resultados cumplen ambos.

   Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
   ```

Copilot abrirá un navegador mediante el servidor Playwright MCP, completará los pasos y comunicará los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más detalles o solicita que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor Playwright MCP quizá tenga que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Después de confirmar que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisarla.

1. Abre el panel **Source Control** de Codespaces y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, por ejemplo, mediante github.com o la interfaz de línea de comandos (CLI) de GitHub. En este caso, pedirás a Copilot que encuentre la incidencia pertinente y cree una PR relacionada con ella.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con los filtros por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y relaciónala con la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede pedirte que te autentiques; sigue las instrucciones.
5. Comprueba cómo Copilot busca las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot y revisa la PR, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es MCP y cómo los servidores MCP amplían las herramientas de Copilot;
- has añadido el servidor Playwright MCP al espacio de trabajo;
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador;
- has publicado la rama y abierto una PR para el trabajo de filtrado.

Has usado MCP para probar una funcionalidad, pero es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
