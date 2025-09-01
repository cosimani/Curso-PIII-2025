.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 08 - PIII 2025
====================
(Fecha: 1 de septiembre)



Práctico Integrador
===================

Consigna
--------

Diseñar un sistema de transmisión digital usando **PAM4** con dos placas **ESP32**,
con posibilidad de integrar un **dsPIC (Easy dsPIC v7)** como generador de datos.
El sistema debe permitir visualizar y analizar en la computadora la señal transmitida
y recibida.

Objetivo general
----------------

- Enviar datos desde un **transmisor (ESP32)** a un **receptor (ESP32)**.
- Implementar una modulación **PAM4** para los datos transmitidos.
- Usar **Wi‑Fi** para enviar resultados a la computadora.
- Visualizar en la PC:
  - formas de onda (tiempo),
  - espectro (**FFT**),
  - **diagrama de ojo**,
  - **latencia** y métricas de calidad (**BER**, **SNR**).

Luego del primer parcial
------------------------

Integrar el **dsPIC** para generar los datos de entrada (por ejemplo,
muestreando una señal analógica con su ADC) y entregarlos al **ESP32** transmisor
(por UART).

Preguntas iniciales para investigar
-----------------------------------

1) ¿Qué es la modulación **PAM4** y cómo se representan los **4 niveles**?
2) ¿Cómo generar símbolos PAM4 en un **ESP32**? ¿Conviene **DAC analógico** o
   **simulación digital** (valores discretos y post‑procesamiento en PC)?
3) ¿Qué limitaciones tienen el **DAC** y el **ADC** del ESP32 (resolución, frecuencia
   de muestreo) y cómo condicionan la **tasa de símbolos**?
4) ¿Cómo se calcula un **diagrama de ojo** y qué información aporta sobre el canal
   (apertura, ISI, jitter)?
5) ¿Qué significa **BER (Bit Error Rate)** y cómo medirlo en este proyecto?
6) ¿Cómo enviar los datos desde el **ESP receptor** a la **PC** en tiempo real por **Wi‑Fi**
   (UDP/TCP/WebSocket)?

Organización por roles (uno por estudiante)
-------------------------------------------

- **HW & Canal**: enlace (analógico o simulado), cableado, niveles de voltaje,
  protección y acondicionamiento (divisores, RC).
- **TX**: generación de símbolos **PAM4** en el transmisor (con **dsPIC** como fuente
  o directamente en **ESP32**), tramas (preambulo/sync/CRC).
- **RX**: muestreo, sincronización (correlación de preámbulo), decisión por umbrales
  adaptativos, recuperación de bits y métricas locales.
- **Visualización**: recepción en PC (Wi‑Fi), graficación (tiempo, **FFT**, **ojo**),
  y cálculo de **BER**, **SNR**, **latencia**, **jitter**.

