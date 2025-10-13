
.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 14 - PIII 2025
====================
(Fecha: 13 de octubre)

Fechas importantes
------------------
- **22 de octubre:** Primer parcial  
- **5 de noviembre:** Segundo parcial  
- **10 de noviembre:** Fecha límite para entrega en GitHub  
- **12 de noviembre:** Último día de clases (recuperatorio y presentación de trabajos o repos incompletos, ya va sin promoción)


Entrega Primer parcial
======================

Objetivo
--------
En esta etapa se evalúa la integración práctica de las placas **dsPIC** y **ESP32** en un sistema de transmisión digital **PAM4**.  

El proyecto se desarrolla en una **primera fase de simulación**, con división de tareas entre los cuatro estudiantes del equipo.


**Descripción general**

Diseñar un sistema de transmisión digital utilizando modulación **PAM4** con dos placas **ESP32**, integrando un **dsPIC (Easy dsPIC v7)** como generador de datos.  
El sistema debe permitir la visualización y análisis de la señal transmitida y recibida en una computadora.


**Etapa 1**

En esta primera fase, todo el sistema se simula con los siguientes componentes y responsabilidades:

Estudiante 1 — Transmisor
-------------------------

- Utiliza la placa **Easy dsPIC v7** con un **dsPIC30F4013**.  
- Muestrea una señal analógica por la entrada **AN2**, con frecuencia aproximada de **1 kHz**.  
- Los datos digitalizados se envían por **UART** hacia una **PC transmisora**.  
- Un programa en **Python** toma los símbolos, realiza la **modulación PAM4** y transmite por **WiFi** (TCP) hacia:

  - El **ESP32 canal**, que simula el medio de transmisión.  
  - Una **laptop de monitoreo**, encargada de visualizar las señales.  

Estudiante 2 — Canal
--------------------

- Implementa un **ESP32** que recibe los datos del transmisor.  
- Simula el canal de transmisión aplicando **ruido** o modificaciones en los bits.  
- Retransmite los datos hacia el **ESP32 receptor** y la **PC de monitoreo**.  
- Utiliza un programa en **Python** para controlar el canal vía **TCP**, permitiendo configurar:

  - Tipo e intensidad del ruido  
  - Parámetros de simulación del canal  

- Gestiona la comunicación entre los dispositivos (IP, puertos y control remoto del canal).

Estudiante 3 — Receptor
-----------------------

- Utiliza otro **ESP32** que recibe las transmisiones del canal.  
- Realiza la **demodulación PAM4**, aplicando umbrales adaptativos para recuperar los bits.  
- Envía los datos demodulados a su **PC receptora** por **Wi-Fi** (TCP).  
- Permite la supervisión de resultados y reenvía datos al módulo de monitoreo.

Estudiante 4 — Monitoreo
------------------------

- Desarrolla una aplicación en **Python (GUI)** que recibe los datos del transmisor, canal y receptor.  
- Muestra en tiempo real:

  - Señales en el dominio del tiempo  
  - FFT y espectro de frecuencia  
  - Diagrama de ojo  
  - BER, SNR, latencia y jitter  

- Permite comparar el rendimiento de todo el sistema y registrar resultados.


Requisitos de la entrega
------------------------

- Todos los temas deben integrarse en una **única entrega colaborativa**.  
- Debe incluir:

  - Código fuente y conexión de hardware.  
  - Puesta en funcionamiento documentada.  
  - Diapositivas o resumen explicativo.  
  - Informe técnico conjunto que integre el trabajo de los cuatro roles.


Evaluación
-----------

- Calificación **8** si todos los temas están correctamente implementados.  
- Aportes adicionales, mediciones experimentales o mejoras en visualización pueden elevar la nota a **9 o 10**.

Fecha de presentación
---------------------

**22 de octubre**


