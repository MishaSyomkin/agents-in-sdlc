---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- pedirás al agente que añada un conmutador de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide también un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio aparece con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, revisa las diferencias en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para revisar el conjunto completo de archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que termine la implementación del modo de alto contraste. Verás un mensaje final que resume los cambios.

## Pide al agente que añada el modo claro

Cuando esté listo el modo de alto contraste, ampliarás la misma conversación con un control para el modo claro. El agente conservará el contexto de su trabajo: los componentes modificados, el enfoque de estilos y el uso del almacenamiento local.

1. Envía este prompt de seguimiento desde el campo de Copilot Chat:

   ```plaintext
   Buen trabajo. Añade ahora un conmutador para el modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón, guardar la preferencia en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el alto contraste de forma independiente.
   ```

2. El agente retomará el trabajo, revisará los cambios realizados y propondrá cómo añadir el modo claro.
3. Revisa cada nueva edición y usa **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de forma que no te convence—, detenlo con **Stop**, junto al campo de chat, y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes (por ejemplo, «simplifica las hojas de estilo en cascada (CSS) y usa variables CSS en vez de duplicar selectores»), corregir el enfoque o ampliar el alcance.

## Revisa los cambios

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Comprueba que se aplican los principios de accesibilidad del agente: atributos de aplicaciones de internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en el [ejercicio siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- has supervisado el trabajo del agente en tiempo real;
- has aceptado y rechazado ediciones de archivos;
- has orientado una sesión en curso mediante prompts de seguimiento.

En el [ejercicio siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios realizados por el agente local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar en el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
