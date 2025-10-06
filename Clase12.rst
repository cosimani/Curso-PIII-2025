.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - PIII 2025
====================
(Fecha: 6 de octubre)


Instalación del compilador, drivers y primer programa
-----------------------------------------------------

Objetivo de la clase
--------------------
- Instalar y configurar el entorno de desarrollo mikroC para dsPIC.
- Verificar la instalación de drivers y comunicación con la placa de desarrollo (Ej: Easy dsPIC v7).
- Realizar el primer programa tipo "Hola Mundo" encendiendo y apagando un LED conectado a un pin digital del dsPIC.
- Cargar el programa en la placa y probar su funcionamiento.


Pasos previos
-------------

1. **Descarga e instalación de mikroC para dsPIC**

   - Sitio oficial: https://www.mikroe.com/mikroc-dspicpic24
   - Descargar e instalar.

2. **Instalación de drivers de la placa de desarrollo**

   - Conectar la placa Easy dsPIC v7 por USB a la computadora.
   - Al conectar explorar las carpetas de instalación para encontrar el driver

3. **Configuración del proyecto en el mikroC**

   - Seleccionar el microcontrolador correspondiente (ej: dsPIC30F4013).
   - Confiogurar el proyecto con XT sin PLL
   - Al compilar, mikroC genera un archivo `.hex` que se debe cargar en el dsPIC con el mismo mikroC.


Primer programa: Parpadear un LED en RB0
----------------------------------------

.. code-block:: c

   void main() {
       TRISBbits.TRISB0 = 0; // Configura RB0 como salida
       LATBbits.LATB0 = 0;   // Inicializa en bajo

       while (1) {
           LATBbits.LATB0 = 1;  // Enciende LED
           Delay_ms(500);       // Espera 500ms
           LATBbits.LATB0 = 0;  // Apaga LED
           Delay_ms(500);       // Espera 500ms
       }
   }



Ejercicio 19:
=============

- Escribir un programa para un dsPIC 4013 para que encienda un led a 3 segundos y otro a 11 segundos.
- Implementar en la placa de desarrollo.

