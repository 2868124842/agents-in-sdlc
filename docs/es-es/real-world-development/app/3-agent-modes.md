---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios de mayor alcance requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para trabajar con el flujo de una organización y ayudar a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo dirigido por agentes: partirás de una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera, y finalmente la integrarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión mientras continúas con el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o PR diferentes para los distintos tipos de archivos, pero seguiremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la nueva funcionalidad con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un lienzo del navegador.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Utilizarás el modo Plan para resolver esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación basados en IA en el flujo de desarrollo no cambia los principios fundamentales. De hecho, son aún más importantes. La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Abrir una incidencia con los detalles del trabajo que se debe realizar.
2. Crear un plan de lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Una vez revisado el código y superado el proceso de integración continua, combinar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría sigue alguna variante del proceso anterior.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el nivel de autonomía del agente. Puedes seleccionarlo en el menú desplegable que hay debajo del campo de indicación y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente colaboráis. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: primero, el agente crea un plan. Lo revisas y apruebas antes de que lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar una respuesta.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de las valoraciones por estrellas se ha combinado y que tu rama local `main` está actualizada.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, en la navegación superior del repositorio, selecciona **Issues**. Abre la incidencia existente **Allow users to filter games by category and publisher**; no hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está vinculada a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si los comandos funcionan en tu terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y, después, ciérrala por completo y vuelve a abrirla. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene el contexto de la solicitud de funcionalidad. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye los filtros por categoría y editorial, controles accesibles, cambios en el acceso a los datos y pruebas. Comenta cualquier comportamiento que no esté claro, por ejemplo, cómo se combinan varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes en el proyecto. Céntrate en implementar y probar los filtros; crearás la PR después de completar el flujo de calidad. Pide que se modifique el plan antes de aprobarlo y guarda la URL de la incidencia y las aclaraciones acordadas para validar la funcionalidad más adelante.

## Aprueba explícitamente Autopilot

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente en tu versión. Comprueba cuál es el modo activo; no des por hecho que la aprobación lo cambia. Confirma que el indicador de modo muestra **Autopilot**.

¡Copilot empezará a implementar la funcionalidad! Verás cómo itera siguiendo el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan antes de aprobarlo. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones. Detén únicamente los servidores que hayas iniciado tú.

## Revisa y verifica la implementación

Una vez generado, hay que revisar el código antes de combinarlo, como cualquier otro cambio. Revisarás el código y ejecutarás el sitio para asegurarte de que todo funciona.

1. Abre **Changes** e inspecciona la implementación de los filtros y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias y E2E, y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este árbol de trabajo y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio en un lienzo del navegador.

1. Utiliza esta indicación para pedir a Copilot que inicie la aplicación y abra la página en el lienzo del navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y comprueba que los resultados coinciden con ella.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las siguientes lecciones

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, su árbol de trabajo y la rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y después añadirás mejoras para la habilidad y el agente de QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor de desarrollo; vuelve a ella en la siguiente lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente desde una incidencia de GitHub.
- definiste los requisitos en modo **Plan**.
- implementaste la funcionalidad con el modo **Autopilot**.
- revisaste el código.
- validaste manualmente la funcionalidad en un lienzo del navegador.

A continuación, profundizarás en la generación de código para asegurarte de que sigue las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
