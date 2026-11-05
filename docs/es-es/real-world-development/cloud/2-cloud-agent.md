---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o funcionalidades que siguen en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, los desarrolladores pueden delegar tareas tediosas y centrarse en otros trabajos.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- proteger las operaciones;
- delimitar claramente las incidencias;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles muestre juegos relacionados —otros títulos de la misma categoría— para que los visitantes puedan seguir explorando. No necesita una implementación refinada todavía; quiere probar la experiencia de usuario (UX). Aunque es un bloqueo, hay otras incidencias con mayor prioridad.

Son tareas que suelen aplazarse y que se pueden asignar al agente de Copilot en la nube. El agente trabaja de forma asíncrona mientras tú te ocupas de otras tareas, y después puedes revisar el trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot crea una PR en borrador para mostrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de terminar. Cuando la solución esté lista para revisarse, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no es magia: sigue siendo necesario pensar. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, [delimita con cuidado el trabajo que asignas al agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

Los ejercicios anteriores han presentado las instrucciones personalizadas. Si ya completaste el ejercicio práctico, habrás visto cómo funciona `.github/copilot-instructions.md`; en caso contrario, este es un buen momento para revisarlo. Antes de asignar trabajo al agente en la nube, inspecciona en modo de solo lectura los archivos de instrucciones del repositorio para reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia **Code lacks documentation**, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas pautas proceden de los archivos de instrucciones y se volverán a mencionar al revisar las PR.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Para generar código, normalmente hay que configurar el entorno y ejecutar scripts de preparación. Esto también se aplica al agente de Copilot. El agente usa [GitHub Actions][github-actions] en su entorno de trabajo.

Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. El archivo está preparado para este taller e instala Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para estos pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso contents: read. Si no se clona el repositorio, Copilot lo hará automáticamente cuando terminen estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación Astro con Drizzle/libSQL y Node.js
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos características importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- Incluye el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio y comprobar que funciona.

## Añade documentación

Muchos proyectos tienen documentación desactualizada o carecen de ella. Esa deuda técnica ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación; por eso, esta es una buena incidencia para asignarle. Más adelante revisarás su trabajo.

1. Abre tu repositorio en github.com en una nueva pestaña.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Sin embargo, las actualizaciones recientes no han seguido este estándar. Necesitamos actualizar el código existente para incluir comentarios de documentación cuando ayuden a aclarar el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Espera a que Copilot cree una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Explora la sesión en directo y observa cómo Copilot explora el código, analiza el estado, planifica e implementa cambios.

El proceso puede tardar varios minutos. Como el agente trabaja de forma asíncrona, puedes asignarle otra tarea mientras te ocupas de otras labores.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente de Copilot en la nube es que permite dividir el trabajo. Mientras el agente trabaja en la documentación, puedes asignarle otra tarea.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en el campo **Title**.
5. Escribe lo siguiente en **Description**:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego, excluyendo ese juego.
   - Mostrar en la página de detalles una sección "Related games" que use el helper.
   - Gestionar el caso en que un juego no tenga juegos relacionados.
   - Crear pruebas unitarias para el nuevo helper.
   - Asegurarse de que todas las pruebas pasan antes de crear la PR.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, después, **Assign**.

Poco después verás varios 👀 en el primer comentario de la incidencia: indican que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en una incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente trabaja en segundo plano, de forma parecida a un ingeniero de software. No tienes que supervisarlo constantemente; revisa el resultado cuando termine. Mientras tanto, explorarás los agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- proteger las operaciones;
- delimitar claramente las incidencias;
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, aprenderás a crear y usar agentes personalizados. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
