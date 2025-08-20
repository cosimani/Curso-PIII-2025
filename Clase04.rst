.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - PIII 2025
====================
(Fecha: 18 de agosto)



Ejercicio 8
============

**Comparación de muestreo sub-Nyquist y sobre-Nyquist**

Objetivo
--------

- Generar una señal compuesta por varias frecuencias.
- Muestrearla a distintas frecuencias de muestreo: una inferior, una igual y una superior a 2·f_max.
- Observar el efecto del aliasing y compararlo visualmente.

Pasos
-----

1) Crear una señal compuesta de 3 senoidales con frecuencias bien separadas (por ejemplo, 300 Hz, 800 Hz, 1.5 kHz) y duración de 1 segundo a una frecuencia de muestreo alta (44.1 kHz).
2) Remuestrear la señal con ``scipy.signal.resample`` a tres valores de ``fs``:
   - ``fs_bajo`` < 2·f_max
   - ``fs_limite`` ≈ 2·f_max
   - ``fs_alto`` > 2·f_max
3) Graficar para cada caso:
   - Señal en el tiempo (zoom a 20 ms).
   - FFT hasta 2 kHz.
4) Discutir en qué casos se observa aliasing y cómo se “mezclan” las frecuencias.




---




--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos y gráficos generados.
2. Usar la carpeta Clase04
3. Documentar en ``README.md`` cada ejercicio.
4. Preparar una carpeta ``utils`` con archivos ``.py`` en los que se agrupen sus funciones útiles.


