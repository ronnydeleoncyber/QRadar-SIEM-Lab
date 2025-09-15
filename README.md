# QRadar-SIEM-Lab
<img width="862" height="431" alt="Captura de pantalla 2025-09-12 133614" src="https://github.com/user-attachments/assets/7181d986-1976-4c7e-a5d5-b5ec1ca70596" />

# Introduccion

La gestión de eventos e información de seguridad (SIEM) es esencial para centralizar y analizar eventos de seguridad en un SOC. Estas plataformas permiten detectar amenazas de forma temprana y mejorar la respuesta ante incidentes.

IBM QRadar SIEM es una de las soluciones más utilizadas a nivel mundial. Gracias a su motor de correlación y capacidades de análisis, ayuda a identificar patrones sospechosos y a obtener una visión integral de la seguridad en la infraestructura.

Este proyecto documenta el uso de IBM QRadar SIEM mediante ejercicios prácticos, con dos objetivos principales:

1. Demostrar experiencia práctica en el manejo de un SIEM empresarial.
    
2. Servir como recurso de consulta para quienes deseen iniciarse en QRadar y en la operación de un SOC.

QRadar SIEM proporciona siete paneles de control preconfigurados.

<img width="870" height="418" alt="image" src="https://github.com/user-attachments/assets/6a46ff4f-e564-4e65-9159-4ae2d61ac142" />


# Vulnerability Management en IBM QRadar SIEM o Gestion de Vulnerabilidades

El panel de gestión de vulnerabilidades en QRadar permite administrar y visualizar información relacionada con las debilidades detectadas en los activos de la red.

En esta práctica:

Revisé la barra de herramientas del panel, que permite:

Crear un nuevo panel de control.

Cambiar el nombre de un panel existente.

Eliminar un panel.

Añadir elementos preconfigurados al panel.


<img width="865" height="323" alt="image" src="https://github.com/user-attachments/assets/a06f2e33-0555-4aef-963f-7cbdf9147804" />


# Creación del panel Watch

Como parte del ejercicio, se creó un panel adicional llamado Watch.

Pasos:

En el menú de vulnerabilidades, seleccioné Nuevo panel.

Ingresé el nombre Watch.

(Opcional) Seleccioné la casilla Compartir, lo que permite que otros usuarios puedan visualizar el panel.

Guardé los cambios haciendo clic en Aceptar.


<img width="866" height="322" alt="image" src="https://github.com/user-attachments/assets/c0b92e2d-3528-4621-94a6-fefbce5b4370" />


El nuevo panel quedó visible, aunque vacío inicialmente.


# Añadiendo elementos al panel

Para enriquecer el panel Watch, se agregó un elemento preconfigurado de sesgo de flujo (Flow Bias):

En la barra de herramientas del panel, seleccioné Añadir elemento.

De la lista de búsquedas predefinidas, elegí Flow Bias.

<img width="877" height="314" alt="image" src="https://github.com/user-attachments/assets/469cfe98-4cc3-4769-a50e-1158253cb2c0" />

Reflexión personal

Este panel resulta útil para identificar desequilibrios en los flujos de red (entrada vs salida), lo que puede ser indicativo de actividades sospechosas o configuraciones anómalas en los activos.


# Uso de un elemento del panel de control

En este ejercicio se utilizó QFlow de IBM QRadar, encargado de recolectar tráfico de red y generar registros de flujos.
Un flujo representa una comunicación entre sockets de red y QRadar lo clasifica según su sesgo de flujo (Flow Bias), lo que ayuda a identificar patrones anómalos o posibles incidentes de seguridad.

Tipos de sesgo de flujo

Out Only (solo salida): intentos de conexión saliente bloqueados, típicos de malware tratando de comunicarse con servidores C2.

<img width="830" height="354" alt="image" src="https://github.com/user-attachments/assets/94188d6b-a377-4a01-918f-b418f829d731" />


In Only (solo entrada): intentos de conexión entrante bloqueados, como escaneos de puertos desde internet.

Mostly Out (principalmente saliente, 70–99%): indica exfiltración de datos, esperable solo en servidores públicos.

<img width="949" height="365" alt="image" src="https://github.com/user-attachments/assets/b0dcb9ce-7fe5-4a36-aca2-cc9821873db6" />


Mostly In (principalmente entrante, 70–99%): típico del tráfico hacia equipos de usuarios.

<img width="902" height="355" alt="image" src="https://github.com/user-attachments/assets/6c124e7b-e092-4eb3-9544-2704a9222ca1" />



Near Same (casi igual, 31–69%): común en VoIP, SSH y chat.

<img width="935" height="357" alt="image" src="https://github.com/user-attachments/assets/911b9693-5080-43ee-880e-8c93278d28f9" />


Other: flujos que no encajan en las categorías anteriores.


Actividades realizadas

Se añadió el gráfico Flow Bias al panel de control Watch.

Se configuró el intervalo de tiempo a Últimos 15 minutos para observar actividad reciente.

Se interactuó con el gráfico:

Zoom en periodos específicos.

Inspección de bytes registrados por categoría (in/out).

Ocultamiento selectivo de sesgos para resaltar comportamientos menos frecuentes.

Se investigó un sesgo específico usando la opción View in Network Activity, lo que abre la consulta en la pestaña de Network Activity.

Observaciones

Los sesgos de flujo inusuales pueden indicar configuraciones incorrectas de la red o incluso una brecha de seguridad.

El gráfico es útil para correlacionar actividad sospechosa con reglas de detección y para validar la jerarquía de red en QRadar.



















