---
title: "Lección 5: Personalizar y usar una habilidad de comprobaciones de calidad"
description: "Explora la habilidad existente quality-checks, personaliza el formato de su informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para asegurarte de que sigue nuestros estándares. Pero ¿qué hay de las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que un agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad que contienen un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción que definen la habilidad, una explicación de lo que hace y pautas sobre cuándo utilizarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad necesite.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos que ya existen en el proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta global de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.
4. Comprueba que la habilidad aún no incluye una sección de formato de los resultados.

## Ejecuta la habilidad antes de cambiarla

Puedes invocar las habilidades directamente desde Copilot CLI o pedirlo en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación sobre los filtros en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejorarás el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué han comunicado realmente las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o ha quedado bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta los comunica. Indica el siguiente paso para cada fallo o bloqueo y nunca presentes como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación. Ejecuta de nuevo la habilidad y compara el informe con el primero.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt de antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la habilidad.

Esta actualización acompañará a los filtros en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
