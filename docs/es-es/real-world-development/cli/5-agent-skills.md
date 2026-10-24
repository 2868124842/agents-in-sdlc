---
title: "Lección 5: Personaliza y utiliza una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué pasa con las pruebas, el lint y el resto de los procesos de integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la mejor opción. Las habilidades ayudan a Copilot a entender cómo ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar y ejecutarás la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad que contiene un archivo Markdown llamado `SKILL.md`. El Markdown incluye metadatos con un nombre y una descripción para definir la habilidad, una explicación de su funcionamiento y orientaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad puede utilizar.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden encontrarse en la carpeta `.github/skills` del proyecto para que sean recursos compartidos y reutilizables por el equipo, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos, llamada `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el proceso de lint.
4. Observa que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Puedes invocar las habilidades directamente desde Copilot CLI o utilizando lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Utiliza esta indicación:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Anota el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si se ha superado y qué han notificado realmente las herramientas. Vamos a actualizar la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Ya has hecho el cambio. Veamos cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin que tengas que reiniciar la conversación.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Utiliza exactamente la misma indicación que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Anota el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente. En esta lección:

- exploraste la habilidad `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar la habilidad y la ejecutaste.

Este cambio se incluirá con el filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
