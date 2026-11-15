---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidad. Aunque está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este panel central puedes ver las tareas de agentes con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y orientar las sesiones que aún están en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería conveniente añadir un modo claro. Como ya se están actualizando los estilos y se está añadiendo una opción para alternar el modo, incluir esta función parece razonable. Guiarás al agente para que añada el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado actual de las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías encontrar las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los distintos pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de PR, que aparece a la derecha y permite ver la solicitud y los archivos modificados.

## Guía al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta indicación:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube en la página de agentes con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la confirma y la incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo de trabajo después de completar el paso en el que estuviera trabajando cuando enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el panel central para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una tarea en curso para ajustar su alcance o añadir indicaciones.

En este ejercicio has explorado:

- la página de agentes y mission control para supervisar las tareas del agente en la nube.
- cómo redirigir una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
