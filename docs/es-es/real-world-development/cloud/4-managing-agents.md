---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que se encargue de tres tareas distintas relacionadas con la mejora de la experiencia de usuario y la incorporación de funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de contraste alto, el equipo decidió que también era un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una función para cambiar de modo, parecía lógico incluirlo. Quieres orientar el trabajo del agente para que añada tanto el modo claro como el contraste alto.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en las listas de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Permite consultar la PR y los archivos modificados para supervisar mejor la tarea.

## Orientar el trabajo del agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe el prompt siguiente:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incluye en su trabajo.

## Dejar que Copilot trabaje

Copilot empezará a trabajar en la tarea actualizada, como antes. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Este proceso también tardará unos minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde Mission Control puedes hacer un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o proporcionar instrucciones adicionales.

Has explorado estos conceptos:

- has usado Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
