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

<img width="872" height="398" alt="image" src="https://github.com/user-attachments/assets/fa08dbbe-d190-453e-a730-be0d695434e3" />



Observaciones

Los sesgos de flujo inusuales pueden indicar configuraciones incorrectas de la red o incluso una brecha de seguridad.

El gráfico es útil para correlacionar actividad sospechosa con reglas de detección y para validar la jerarquía de red en QRadar.



# Investigacion de una infraccion de acceso remoto

Este caso muestra cómo QRadar detecta un intento de acceso remoto mediante RDP desde Internet y cómo un analista SOC realiza el proceso de investigación y triage.

1. Detección de la infracción

En la pestaña Offenses (Infracciones) se genera una alerta con el nombre:
“Acceso a Escritorio remoto desde Internet – RemoteAccess.MSTerminal Services”.

<img width="565" height="253" alt="image" src="https://github.com/user-attachments/assets/87d4996b-80de-40c5-a565-11a5399b7d35" />


2. Revisión de la regla correlacionada

La infracción está basada en una regla predefinida que considera:

Tráfico RDP (3389) desde un origen externo.

Comunicación remota → local.

Bloques de construcción (BB) de Remote Access Violation y Successful Communication

<img width="863" height="388" alt="image" src="https://github.com/user-attachments/assets/45bd5555-1b58-4b2f-ac2f-01ba41aa8151" />

Mas detallado

<img width="868" height="389" alt="image" src="https://github.com/user-attachments/assets/c4dc265e-cc10-4df4-852c-c5747edf91b1" />


3. Acciones de la regla

Por defecto la regla:

Agrupa la infracción por IP de origen (atacante).

Genera un evento adicional de acceso remoto.

Opcionalmente, se podrían configurar notificaciones o escaneos.

<img width="891" height="392" alt="image" src="https://github.com/user-attachments/assets/459d7367-e2b3-46ae-a867-86b876008072" />


5. Conclusión del análisis

El intento de conexión RDP desde Internet es un indicador claro de acceso remoto no autorizado.

QRadar ayuda a priorizar y correlacionar la información, permitiendo al analista confirmar la amenaza y decidir la acción de respuesta (bloqueo, escaneo forense, escalamiento).




# Creación de una búsqueda de conexiones RDP a su servidor


Acceso al módulo de búsqueda

Aquí es posible crear consultas personalizadas con filtros para protocolos, direcciones IP, puertos, etc. ( EN ESTE CASO APLICAMOS EL FILTRO EN BASE A LA IP 


<img width="896" height="429" alt="image" src="https://github.com/user-attachments/assets/3f05538e-e60f-4faa-882f-af0197a7b175" />



Ejecución de la búsqueda

Al ejecutar la búsqueda, QRadar devuelve los flujos/eventos que cumplen los criterios.

Se revisa origen, destino y hora de la conexión para identificar accesos sospechosos.

<img width="793" height="305" alt="image" src="https://github.com/user-attachments/assets/e929ed1f-7cb5-4d47-880b-69b81f1fe3fe" />

( MAS DETALLE DEABAJO )



# Investigación de un delito de acceso remoto con la aplicación Analyst

Este ejercicio se centra en la aplicación Analyst de QRadar, utilizada para gestionar investigaciones de incidentes de seguridad.
A diferencia de la vista tradicional de Offenses, aquí se estructura la información en un caso investigativo, con contexto y evidencias asociadas.


1. Identificación del incidente

Se recibe una infracción relacionada con acceso remoto sospechoso (RDP).

En lugar de investigarla únicamente desde la pestaña Offenses, se abre el caso en la aplicación Analyst para documentar la investigación.

<img width="847" height="383" alt="image" src="https://github.com/user-attachments/assets/eb5e1b56-1b38-40bf-9125-3e8e8814f5fa" />


2. Uso de la aplicación Analyst

Dentro de Analyst App se pueden:

Revisar detalles del incidente (regla que lo disparó, IP origen/destino, hora, etc.).

Asociar eventos y flujos relacionados con la infracción.

Añadir notas o comentarios de análisis.

<img width="830" height="405" alt="image" src="https://github.com/user-attachments/assets/83bec1e0-0192-4e13-8961-839c0f9e18ad" />


Esto ayuda a entender si el intento de acceso remoto forma parte de un ataque aislado o de un patrón más amplio.



Conclusión de la investigación

El uso de la aplicación Analyst permite dar trazabilidad y formalidad a la investigación del incidente.

Un analista SOC puede documentar evidencias, registrar hipótesis y preparar el caso para escalamiento o cierre.




# Creación de una plantilla de informe de acceso remoto

La investigación de incidentes en un SOC no está completa hasta que se genera un informe formal.
En QRadar, es posible crear plantillas de reportes que reúnen evidencias y gráficos clave para documentar infracciones, apoyar auditorías y facilitar la comunicación entre equipos de seguridad.


1. Acceso al módulo de reportes

Desde la pestaña Reports, se crea una nueva plantilla.

Se selecciona el tipo de informe: Acceso remoto sospechoso (RDP).

<img width="858" height="326" alt="image" src="https://github.com/user-attachments/assets/35d5ec5b-38cf-4813-a604-82ef9e83ec77" />


2. Configuración de la plantilla


v<img width="857" height="397" alt="image" src="https://github.com/user-attachments/assets/38d27454-b2c8-4673-8e8b-4390075a168b" />


Reporte que genere

<img width="863" height="392" alt="image" src="https://github.com/user-attachments/assets/8e9be6a0-9a26-49eb-8392-1980ba153ea3" />


<img width="881" height="390" alt="image" src="https://github.com/user-attachments/assets/d6eddb15-65a1-4308-8cfa-38e550a66ee1" />


<img width="871" height="394" alt="image" src="https://github.com/user-attachments/assets/783d021f-e483-43f5-b5ed-89dd50a2206d" />



Este laboratorio de IBM QRadar SIEM permitió reforzar habilidades clave en la operación de un SOC, desde la exploración de paneles y gestión de vulnerabilidades, hasta la investigación de incidentes de acceso remoto y la generación de reportes formales.

A través de los ejercicios documentados se demostró la capacidad de:

Detectar y analizar eventos de seguridad (ej. intentos de acceso remoto vía RDP).

Investigar incidentes utilizando diferentes módulos de QRadar (QFlow, Analyst App).

Crear búsquedas personalizadas y reportes que facilitan la comunicación y el cumplimiento de auditorías.

Con este proyecto no solo se obtiene experiencia práctica en el uso de un SIEM empresarial, sino que también se construye evidencia tangible para el desarrollo profesional en ciberseguridad y respuesta a incidentes.

















