---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes con solicitudes de incorporación de cambios (PR) abiertas. Puedes explorar las operaciones realizadas y dirigir una sesión en curso para orientar el trabajo.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión activa y pedirás una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya estaba modificando los estilos del sitio y añadiendo controles de alternancia, parecía lógico incluir esta funcionalidad. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberían aparecer las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque elegido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Te permite ver la PR y los archivos modificados para supervisar el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto variará según el nombre elegido por Copilot y el estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes similares a los ejemplos.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añade también un modo claro. Debe haber un conmutador para este modo que permita a los usuarios elegir cómo quieren ver el sitio.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para dirigir la sesión activa mientras Copilot trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud después de completar el paso en el que estaba trabajando cuando enviaste el mensaje.

El proceso tardará varios minutos, así que puedes hacer una pausa y reflexionar sobre lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, un centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- utilizaste el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión activa para solicitar una funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
