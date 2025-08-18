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

Ejercicio 9
============

**Cuantificación y distorsión por granularidad**

Objetivo
--------

- Implementar un cuantificador uniforme de ``Nbits`` configurable.
- Comparar la señal original y la cuantificada, tanto en el dominio del tiempo como en el espectro.

Pasos
-----

1) Generar una onda seno de 440 Hz, amplitud 1.0 y duración 2 segundos a 44.1 kHz.
2) Implementar una función ``cuantificar`` que reciba los parámetros para la cuantificación y que devuelva la señal cuantificada.
3) Probar con distintos ``Nbits`` (8, 4, 2) y graficar:
   - Señal original vs. cuantificada (zoom a unas pocas ondas).
   - Error de cuantificación.
4) Calcular la SNR aproximada y compararla con la fórmula teórica:  
   ``SNR ≈ 6.02*Nbits + 1.76 dB``.

---





--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos y gráficos generados.
2. Usar la carpeta Clase04
3. Documentar en ``README.md`` cada ejercicio.
4. Preparar una carpeta ``utils`` con archivos ``.py`` en los que se agrupen sus funciones útiles.


