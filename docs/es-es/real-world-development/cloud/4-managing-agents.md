---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar el trabajo asignado al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este espacio de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión activa y pedirás una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería útil añadir un modo claro. Como ya se están actualizando los estilos y se está añadiendo un conmutador, pedirás al agente que incluya también el modo claro.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas que aparecen en los paneles principal e izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar interfaces de programación de aplicaciones (API) para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo;
   - configurar servidores MCP;
   - realizar distintas tareas para completar el trabajo.

> [!NOTE]
> Los pasos exactos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Revisa también el panel de PR, que aparece a la derecha y permite ver la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que añada el modo claro mientras implementa el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Ya que estamos implementando un modo de alto contraste, añadamos también un modo claro. Debe haber un conmutador para este modo, para que cada persona pueda elegir cómo quiere ver el sitio.
   ```

   ![Tarea del agente en la nube con el diálogo de orientación de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot reconoce la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje. El proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el espacio central para supervisar y orientar las tareas del agente en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- has usado mission control y la página de agentes para supervisar tareas;
- has redirigido una sesión activa para pedir una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar en las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar en el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
