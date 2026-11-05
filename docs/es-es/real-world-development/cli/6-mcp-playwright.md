---
title: "Lección 6: Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para probar los filtros en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos comentado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y habilitar automatizaciones adicionales para Copilot. Para ello sirven los servidores de Model Context Protocol (MCP): permiten que Copilot amplíe las herramientas y los servicios integrados en la CLI.

En esta lección:

- entenderás qué es MCP y cómo lo usa Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore los filtros.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Permitirás que Copilot use el sitio web como lo haría una persona y automatice más comprobaciones para aumentar la confianza en el resultado.

## ¿Qué es MCP?

[MCP][mcp-blog-post] permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente y los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las interfaces de programación de aplicaciones (API) para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa en la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el diálogo para añadir un servidor MCP introduciendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora pedirás a Copilot que haga lo mismo con el servidor Playwright MCP.

1. Pide a Copilot que use el servidor para validar la funcionalidad:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar los filtros según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que inicies cuando termines.
   ```

> [!NOTE]
> No es imprescindible pedir a Copilot que use el servidor MCP: normalmente lo detectará por sí mismo. Indicarle la herramienta puede hacer que los resultados sean más coherentes y reducir el uso de tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo funciona según lo esperado.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has utilizado el servidor Playwright MCP para explorar tu funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo usa Copilot CLI;
- has añadido el servidor Playwright MCP si no estaba disponible;
- has pedido al agente que controlara un navegador y explorara los filtros.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
