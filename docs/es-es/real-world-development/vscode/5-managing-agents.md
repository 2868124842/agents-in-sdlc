---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo tiene lugar en tu equipo y en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las modificaciones de archivos mientras se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar los cambios y orientar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las modificaciones propuestas.
- orientarás al agente durante la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación desde cero, ampliarás el trabajo en la sesión existente.

## Supervisa el progreso de Copilot

La vista de Copilot Chat muestra cada paso que realiza el agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Las modificaciones aparecen en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista de Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cómo llegan las llamadas a herramientas. Verás lecturas y ediciones de archivos, además de solicitudes para ejecutar comandos del terminal.
3. Cuando el agente proponga una edición, aparecerá una diferencia en línea:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para deshacer el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo para ejecutar un comando del terminal, el agente se detendrá y mostrará la opción **Allow**. Autoriza solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final debería mostrar un resumen de los cambios.

## Dirige al agente para que añada el modo claro

Ya está implementado el modo de alto contraste. Ahora ampliarás la misma conversación para añadir un control de modo claro. Como continúas en el mismo chat, el agente conserva el contexto del trabajo previo, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Buen trabajo. Ahora añade un control para el modo claro junto al control de alto contraste. Sigue el mismo patrón: guarda la preferencia en el almacenamiento local y permite acceder al control desde la misma zona de la interfaz. Los usuarios deben poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente retomará el trabajo, explorará los cambios que ya ha realizado y propondrá modificaciones para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente se desvía del enfoque deseado, por ejemplo, si intenta refactorizar la interfaz de los controles de una forma que no te gusta, puedes detenerlo con el botón **Stop** junto al cuadro de entrada y enviar un prompt aclaratorio.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios del agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento local para conservar las preferencias.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión activa: revisaste cada modificación cuando apareció, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste modificaciones de archivos individuales.
- dirigiste la sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
