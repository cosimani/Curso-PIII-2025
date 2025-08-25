.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PIII 2025
====================
(Fecha: 26 de agosto)



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




Simple Moving Average Filter (Media móvil simple)
==================================================

- La ecuación en diferencia para este filtro es la siguiente:

  y[n] = y[n-1] + (1 / W) * (x[n] - x[n-W])

- ``W`` es el tamaño de la ventana deslizante.
- ``y[n]`` es la salida actual.
- ``y[n-1]`` es la salida anterior.
- ``x[n]`` es la entrada actual.
- ``x[n-W]`` es la entrada que sale de la ventana.

- Este filtro actúa como un filtro pasabajos, suavizando la señal al promediar un número fijo de muestras y atenuar los cambios rápidos.




Ejercicio 13
============

- implementar este filtro con distintos tamaños de ventana.
- Comparar con el Media móvil exponencial
- Realizar un análisis del espectro de frecuencias.
- Plotear la señal original y la filtrada superpuestas para notar el suavizado.






Tonos de Shepard
================

- Acústica, psicoacústica, y procesamiento de señales. 
- Los Tonos de Shepard son una serie de tonos puros que, cuando se reproducen en secuencia, crean la ilusión auditiva de un tono que sube o baja infinitamente en altura, a pesar de que en realidad no ocurre un cambio real en la frecuencia.
- Los Tonos de Shepard consisten en varios tonos sinusoidales espaciados por octavas. Al variar la amplitud de estos tonos de forma que los extremos (frecuencias más bajas y más altas) se atenúan, se crea una ilusión de un tono que parece subir o bajar infinitamente.
- Psicoacústica: La percepción de los Tonos de Shepard explota una característica de nuestro sistema auditivo: la capacidad de identificar tonos en diferentes rangos de frecuencia, pero sin una clara distinción cuando los tonos están compuestos de múltiples frecuencias.


.. code-block:: python

	import numpy as np
	import matplotlib.pyplot as plt
	from IPython.display import Audio

	def generate_shepard_tone(base_freq, num_tones, duration, sample_rate):
	    t = np.linspace(0, duration, int(sample_rate * duration), endpoint=False)
	    tone = np.zeros_like(t)

	    for i in range(num_tones):
	        freq = base_freq * (2 ** i)
	        amplitude = np.sin(np.pi * i / num_tones) ** 2
	        tone += amplitude * np.sin(2 * np.pi * freq * t)

	    return tone

	# Parámetros
	base_freq = 440  # Frecuencia base en Hz (Nota La)
	num_tones = 6    # Número de tonos superpuestos
	duration = 0.5   # Duración de cada tono en segundos
	sample_rate = 44100  # Tasa de muestreo

	# Generar el tono de Shepard
	shepard_tone = generate_shepard_tone(base_freq, num_tones, duration, sample_rate)

	# Reproducir el tono generado
	Audio(shepard_tone, rate=sample_rate)

	def generate_shepard_scale(base_freq, num_tones, duration, sample_rate, steps):
	    scale = []
	    for step in range(steps):
	        tone = generate_shepard_tone(base_freq * (2 ** (step / steps)), num_tones, duration, sample_rate)
	        scale.append(tone)
	    return np.concatenate(scale)

	# Parámetros adicionales
	steps = 100  # Número de pasos en la escala (equivalente a una octava)

	# Generar la escala de Shepard
	shepard_scale = generate_shepard_scale(base_freq, num_tones, duration, sample_rate, steps)

	# Reproducir la escala
	Audio(shepard_scale, rate=sample_rate)



Ejercicio 14
============

- Modificar parámetros como num_tones, base_freq, y steps para observar cómo cambia la percepción.
- ¿Qué ocurre si cambia la tasa de muestreo o la duración de cada tono?
- Implementar una versión que genera una escala descendente.
- Graficar señal, espectro de frecuencias y espectograma.




----

--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos y gráficos generados.
2. Usar la carpeta Clase06
3. Documentar en ``README.md`` cada ejercicio.
4. Continuar con la preparación de la carpeta ``utils`` con archivos ``.py`` en los que se agrupen funciones útiles.


