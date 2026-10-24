---
title: "Lección 1: Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo —cada uno en su propio espacio de trabajo aislado— en lugar de hacer todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto preparada; el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- conocerás el espacio de trabajo, incluida la lista de trabajo pendiente que ha creado la plantilla.
- probarás un chat rápido para obtener información sobre la aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para gestionar un backlog cada vez mayor. La aplicación Copilot ofrece un lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones disponibles para Windows, macOS y Linux. Instalarás la aplicación, te autenticarás y añadirás el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración se te ofrece seleccionar un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina la configuración.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta añadirlo otra vez.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En el diseño que se muestra aquí, las carpetas del repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza las funciones en varias áreas de la barra lateral:

- **New**: inicia una sesión nueva de chat con Copilot.
- **Pull requests**: solicitudes de incorporación de cambios disponibles mediante la integración nativa de GitHub. Puedes examinarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: incidencias disponibles de la misma forma. Puedes examinarlas y filtrarlas, e iniciar una sesión desde una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven para gestionar listas de tareas, realizar mantenimiento periódico y delegar otras tareas repetitivas. El resumen final las presenta como un siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no aparece, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas del repositorio y sus sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión para volver a esa conversación. Cada sesión tiene su propio espacio de trabajo aislado, por lo que puedes ejecutar varias a la vez sin que los cambios entren en conflicto. Iniciarás la primera sesión al añadir valoraciones por estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Puede orientarte si no sabes cómo hacer algo o si es posible.

### Encuentra el backlog inicial

Es probable que todos los proyectos tengan un backlog, y Tailspin Toys no es una excepción. Explora las incidencias que se crearon al crear la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por título, sin dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Después de un primer cambio rápido, empezarás por la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran automáticamente elementos de los repositorios añadidos a la aplicación Copilot. Para ver el trabajo de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es utilizarla para conocer la propia aplicación. Un **chat rápido** es la herramienta adecuada: puedes hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo. Es ideal para una consulta breve que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si aún no aparece **Chats**, selecciona **+** junto a **Sessions** para iniciar tu primer chat; **Chats** aparecerá después.
2. Pregunta a la aplicación cómo funcionan sus propias sesiones:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que los cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y siguientes pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- utilizar un chat rápido para formular una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] para mostrar la valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
