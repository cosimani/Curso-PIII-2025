.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PIII 2025
====================
(Fecha: 25 de agosto)



Ejercicio 12
============

**Multiplicación de señales: modulación de amplitud simple (AM-DSB)**

- Implementar una modulación AM básica multiplicando una señal de mensaje por una portadora.

1) Crear una señal de mensaje ``m(t)``: seno de 200 Hz, amplitud 1.0.
2) Crear una señal portadora ``c(t)``: seno de 5 kHz, amplitud 1.0.
3) Calcular la señal modulada ``y(t) = (1 + m(t)) * c(t)``.
4) Graficar:
   - Mensaje en el tiempo.
   - Portadora en el tiempo (zoom).
   - Señal modulada.
   - Espectro de la modulada (FFT hasta 8 kHz).


- Probar con distintos índices de modulación (cambiar la amplitud de ``m(t)``).
- Observar la distorsión cuando el índice de modulación es > 1 (sobremodulación).

Extras: Graficar todas estas:
======

- AM-DSB con portadora (AM-DSB-FC)
- AM-DSB sin portadora (AM-DSB-SC)
- AM con banda lateral única (SSB - Single Side Band)
- SSB con portadora suprimida (SSB-SC)



----

--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos y gráficos generados.
2. Usar la carpeta Clase06
3. Documentar en ``README.md`` cada ejercicio.
4. Continuar con la preparación de la carpeta ``utils`` con archivos ``.py`` en los que se agrupen funciones útiles.


