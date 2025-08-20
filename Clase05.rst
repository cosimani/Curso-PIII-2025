.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 05 - PIII 2025
====================
(Fecha: 20 de agosto)




Espectrograma (ipynb)
=====================

`Abrir en Colab <https://colab.research.google.com/drive/13pAwFsh85rQo9aO9sPgh1W9acqZMRXBW?usp=sharing>`_


.. code-block:: python
   
   # **Espectrograma**
   # Representación en tres dimensiones: tiempo, frecuencia y amplitud.

   import numpy as np
   import matplotlib.pyplot as plt
   plt.style.use( 'seaborn-darkgrid' )
   from scipy import signal
   from scipy.fft import fft, fftshift
   from scipy.io import wavfile

   # Commented out IPython magic to ensure Python compatibility.
   # %%capture
   # !pip install ffmpeg-python==0.2.0
   #

   segundos = 2
   t = np.linspace( 0, segundos, 44100 * segundos )
   w = signal.chirp( t, f0 = 1, f1 = 1500, t1 = segundos, method = 'quadratic' )
   plt.plot( t, w )
   plt.xlim( [ 0, 1 ] )
   plt.xlabel( 't (sec)' )
   plt.show()

   def cosenoidal( frecuencia, duracion = 1, sample_rate = 44100, A = 1 ) :
       n = np.linspace( 0, duracion, sample_rate * duracion )
       return A * np.cos( 2 * np.pi * frecuencia * n )

   def generador_de_tono( frecuencia, duracion = 1, sample_rate = 44100, A = 1 ) :
       n = np.linspace( 0, duracion, sample_rate * duracion )
       return A * np.sin( 2 * np.pi * frecuencia * n )

   def fourier_calculation( y, sample_rate = 44100 ) :
       '''Aplicando fast fourier transform'''
       yf = fft( y )
       N = len( y )
       yf = 2.0 / N * np.abs( yf[ 0 : N // 2 ] )  # // es la división entera (https://www.youtube.com/watch?v=NRX6KvEP-u8)
       xf = np.linspace ( 0.0, 1 / 2 * sample_rate, N // 2 )
       return yf, xf    

   def plot_tiempo_frecuencia_espectrograma( y, 
                                             sample_rate = 44100, 
                                             tiempo_max = -1,
                                             f_max_fft = 3000,
                                             f_max_espectrograma = 3000 ) :
       '''Plotea la señal en dominio del tiempo, en dominio de la frecuencia y espectrograma'''
       
       # Creo el espacio para plotear, una figura vacía
       plt.figure( figsize = ( 15, 15 ) )
       x = np.linspace( 0, len( y ) / sample_rate, len( y ) )
       
       plt.subplot( 3, 1, 1 )  # Dividido en 2 filas y 1 columna, ploteo la onda en la primer fila
       if tiempo_max != -1 :
           plt.xlim( [ 0, tiempo_max ] )
       plt.plot( x, y )
       plt.grid()  # Grilla de fondo
       
       # FFT
       yf, xf = fourier_calculation( y, sample_rate )
       plt.subplot( 3, 1, 2 )
       plt.xlabel( 'Freq (Hz)' )  
       plt.ylabel( '|Y(freq)|' )
       plt.xlim( [ 0, f_max_fft ] )
       plt.plot( xf, yf )
       
       # Espectrograma
       plt.subplot( 3, 1, 3 )
       a = plt.specgram( y, Fs = sample_rate, cmap = 'jet' )
       plt.ylim( [ 0, f_max_espectrograma ] )
       plt.ylabel( 'Frequency [Hz]' )
       plt.xlabel( 'Time [sec]' )
       plt.colorbar()
       plt.show()

   plot_tiempo_frecuencia_espectrograma( w, sample_rate = 44100, tiempo_max = 0.8 )

   cos_1 = cosenoidal( 300, 5, 44100 )
   cos_2 = cosenoidal( 900, 5 )
   sin_1 = generador_de_tono( 1500, 5, 44100 )
   suma_de_senales = cos_1 + cos_2 + sin_1

   plot_tiempo_frecuencia_espectrograma( suma_de_senales, sample_rate = 44100, tiempo_max = 0.02 )

   """
   To write this piece of code I took inspiration/code from a lot of places.
   It was late night, so I'm not sure how much I created or just copied o.O
   Here are some of the possible references:
   https://blog.addpipe.com/recording-audio-in-the-browser-using-pure-html5-and-minimal-javascript/
   https://stackoverflow.com/a/18650249
   https://hacks.mozilla.org/2014/06/easy-audio-capture-with-the-mediarecorder-api/
   https://air.ghost.io/recording-to-an-audio-file-using-html5-and-js/
   https://stackoverflow.com/a/49019356
   """
   from IPython.display import HTML, Audio
   from google.colab.output import eval_js
   from base64 import b64decode
   import numpy as np
   from scipy.io.wavfile import read as wav_read
   import io
   import ffmpeg

   AUDIO_HTML = """
   <script>
   var my_div = document.createElement("DIV");
   var my_p = document.createElement("P");
   var my_btn = document.createElement("BUTTON");
   var t = document.createTextNode("Press to start recording");

   my_btn.appendChild(t);
   //my_p.appendChild(my_btn);
   my_div.appendChild(my_btn);
   document.body.appendChild(my_div);

   var base64data = 0;
   var reader;
   var recorder, gumStream;
   var recordButton = my_btn;

   var handleSuccess = function(stream) {
     gumStream = stream;
     var options = {
       //bitsPerSecond: 8000, //chrome seems to ignore, always 48k
       mimeType : 'audio/webm;codecs=opus'
       //mimeType : 'audio/webm;codecs=pcm'
     };            
     //recorder = new MediaRecorder(stream, options);
     recorder = new MediaRecorder(stream);
     recorder.ondataavailable = function(e) {            
       var url = URL.createObjectURL(e.data);
       var preview = document.createElement('audio');
       preview.controls = true;
       preview.src = url;
       document.body.appendChild(preview);

       reader = new FileReader();
       reader.readAsDataURL(e.data); 
       reader.onloadend = function() {
         base64data = reader.result;
         //console.log("Inside FileReader:" + base64data);
       }
     };
     recorder.start();
     };

   recordButton.innerText = "Grabando con el micrófono... pulsar para finalizar";

   navigator.mediaDevices.getUserMedia({audio: true}).then(handleSuccess);


   function toggleRecording() {
     if (recorder && recorder.state == "recording") {
         recorder.stop();
         gumStream.getAudioTracks()[0].stop();
         recordButton.innerText = "Guardando la grabación... ¡espere!"
     }
   }

   // https://stackoverflow.com/a/951057
   function sleep(ms) {
     return new Promise(resolve => setTimeout(resolve, ms));
   }

   var data = new Promise(resolve=>{
   //recordButton.addEventListener("click", toggleRecording);
   recordButton.onclick = ()=>{
   toggleRecording()

   sleep(2000).then(() => {
     // wait 2000ms for the data to be available...
     // ideally this should use something like await...
     //console.log("Inside data:" + base64data)
     resolve(base64data.toString())
     recordButton.innerText = "Listo"

   });

   }
   });
         
   </script>
   """

   # Esta función devuelve dos variables
   # audio = son las muestras del audio capturado por el microfóno que identifica nuestro navegador
   # sr = frecuencia de muestro en la que fue capturado el audio 
   def get_audio() :
     display( HTML( AUDIO_HTML ) )
     data = eval_js( "data" )
     binary = b64decode( data.split(',')[1])
     
     process = (ffmpeg
       .input('pipe:0')
       .output('pipe:1', format='wav')
       .run_async(pipe_stdin=True, pipe_stdout=True, pipe_stderr=True, quiet=True, overwrite_output=True)
     )
     output, err = process.communicate(input=binary)
     
     riff_chunk_size = len(output) - 8
     # Break up the chunk size into four bytes, held in b.
     q = riff_chunk_size
     b = []
     for i in range(4):
         q, r = divmod(q, 256)
         b.append(r)

     # Replace bytes 4:8 in proc.stdout with the actual size of the RIFF chunk.
     riff = output[:4] + bytes(b) + output[8:]

     sr, audio = wav_read( io.BytesIO( riff ) )

     return audio, sr

   grabacion, sample_rate = get_audio()

   print( f"Cantidad de canales = { len( grabacion.shape ) }")
   print( f"Cantidad de muestras = { grabacion.shape[ 0 ] }")

   length = grabacion.shape[ 0 ] / sample_rate
   print( f"Duración = { length } segundos" )

   plot_tiempo_frecuencia_espectrograma( grabacion, sample_rate )




Ejercicio 9
===========

- Subir un archivo de audio propio, publicar la frecuencia de muestreo, duración y cantidad de canales, y plotear las tres gráficas con ``plot_tiempo_frecuencia_espectrograma``.
- Si desea descargar un archivo WAV, puede `extraerlo desde un video en Youtube <https://loader.to/es16/youtube-wav-converter.html>`_ .
- Averiguar los bits del ADC, es decir, en cuántos bits está codificada cada muestra.
- Reproducir el archivo .wav
- Realizarlo ahora con archivos mp3. 

----


Ejercicio 10
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

----





Exponential Moving Average Filter (Media móvil exponencial)
===========================================================

- La ecuación en diferencia para este filtro es la siguiente:

.. figure:: images/formula_ema.png

- alpha está en el intervalo ``[0, 1]`` y es el factor de suavizado
- ``y[n]`` es la salida actual
- ``y[n - 1]`` es la salida anterior
- ``x[n]`` es la entrada actual

Ejemplos con distintos valores de factor de suavizado
-----------------------------------------------------

.. figure:: images/ema_alpha_01.png

.. figure:: images/ema_alpha_02.png

- Se llama exponencial porque el factor de ponderación aplicado a las entradas pasadas disminuye exponencialmente. Lo podemos visualizar sustituyendo las salidas anteriores.

.. figure:: images/sumatoria_ema.png

----

Ejercicio 11
------------

- Capturar con el micrófono algunos segundos.
- Definir una función que aplique este filtro de media móvil exponencial.
- Suavizar la señal con un factor de 0.6, 0.2 y 0.05 y visualizar el resultado.
- Realizar un análisis del espectro de frecuencias.
- Plotear la señal original y la filtrada superpuestas para notar el suavizado.


----

--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos y gráficos generados.
2. Usar la carpeta Clase05
3. Documentar en ``README.md`` cada ejercicio.
4. Continuar con la preparación de la carpeta ``utils`` con archivos ``.py`` en los que se agrupen funciones útiles.






