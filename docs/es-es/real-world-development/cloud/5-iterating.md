---
title: "Ejercicio 5: Iterar en el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, desarrollara una función de juegos relacionados y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisarás los cambios y, si es necesario, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de permitir que el resto del equipo revise los cambios, inspeccionarás las propuestas de Copilot para la documentación, los juegos relacionados y la accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión directa, por lo que existen medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su trabajo.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y temporal.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de ejecutarlo.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) creada por el agente: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que Copilot está bloqueado por un firewall. Es lo esperado, ya que el acceso del agente a recursos externos y servidores MCP está limitado de forma predeterminada. Si quieres, puedes [configurar o deshabilitar el firewall del agente][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá nuevos comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario establecidos por los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro terminarán correctamente; puede tardar varios minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay un texto de prompt único; puedes pedir, por ejemplo:

- añadir encabezados breves al principio de cada archivo de código;
- añadir comentarios TSDoc a archivos TypeScript y Astro;
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot** como mencionarías a otro miembro del equipo. Usa una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás cómo inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisarás la PR creada por Copilot para la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR y que las comprobaciones de la aplicación Astro terminan correctamente.
9. **Opcional:** cambia a esta rama en tu Codespace y prueba manualmente la funcionalidad. En el terminal, ejecuta estos comandos y sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes pedirle cambios en los comentarios de la pestaña **Conversation** y Copilot iniciará otra sesión. Por ejemplo, podrías cambiar el número de juegos relacionados o pedir encabezados de comentario y comentarios TSDoc. Recuerda que esta incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado en mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los controles de interfaz para cambiar de modo;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas de aplicaciones de internet enriquecidas accesibles (ARIA), el lenguaje de marcado de hipertexto (HTML) semántico y la navegación con teclado;
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR y terminan correctamente.
10. **Opcional:** cambia a la rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ejecuta los comandos siguientes y sustituye `<branch-name>` por el nombre de la rama de Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones según las buenas prácticas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para asignarle tareas reales del backlog mientras te ocupas de otras cosas. Para practicar, crea más incidencias y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés de patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de listado de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado e iterado en las PR del agente**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Para seguir explorando, puedes probar otros entornos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el entorno de desarrollo integrado (IDE).
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades, un agente de control de calidad (QA), Playwright MCP, Agent Merge y la opción `/delegate` para volver al agente en la nube.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
