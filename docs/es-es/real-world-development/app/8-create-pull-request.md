---
title: "Lección 8: Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de las instrucciones y la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ha llegado el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios (PR). Tú combinaste la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, hemos mantenido todo el flujo de filtrado y calidad en una misma sesión y rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás la PR completa de la funcionalidad y las evidencias de verificación.
- autorizarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR, pero mantener en manos del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la PR y resuelve los obstáculos —corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y actualiza la base de la rama mediante un rebase cuando hace falta—, y la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, permanece activo aunque se reinicie la aplicación y se desactiva al combinarse la PR.

Hasta ahora, tú mismo has seleccionado **Merge pull request**. Agent Merge puede encargarse de ello, pero para editar código y combinar la PR sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora permitirás que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agente para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia.

Se iniciará el proceso de Agent Merge. El agente:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, la incluirá como referencia en el cuerpo de la descripción.
- Actualizará la base de la rama mediante un rebase o resolverá posibles conflictos de combinación con la rama de destino.
- Supervisará CI para asegurarse de que se superen todas las comprobaciones.
- Supervisará los comentarios de otros desarrolladores o de la revisión de código de Copilot en la PR y realizará cambios para resolverlos.
- Opcionalmente, podrá combinar la PR automáticamente cuando todo se haya completado correctamente.

Permitiremos que Agent Merge combine también la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las evidencias de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve cualquier obstáculo que se comunique. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso para combinar o un agente en ejecución no confirma que la combinación haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que faltan. Resuelve esos obstáculos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste la PR completa de la funcionalidad y las evidencias de verificación.
- autorizaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [utilizarás un lienzo existente y crearás un lienzo de triaje][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
