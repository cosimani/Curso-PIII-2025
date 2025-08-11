.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 01 - PIII 2025
====================
(Fecha: 6 de agosto)

:Profesor: César Osimani
:Mail: cesarosimani@gmail.com

:Temas principales:
	- Programación a bajo y alto nivel para el desarrollo de aplicaciones de telecomunicaciones
	- Herramientas de programación de hardware
	- Resolución de problemas con la programación
	- Dispositivos programables
	- Tratamiento de señales en sistemas embebidos
	- Técnicas de programación para sistemas de comunicaciones


:Regularidad: 
	- Primer y segundo parcial (Guía de trabajos prácticos)

:Examen final: 
	- Presentación de un proyecto integrador





Instalación de herramientas
===========================

:Python: 
	- Descargamos e instalamos `Python 3.8.10 <https://www.python.org/downloads/release/python-3810/>`_ 
	- Elegir instalador para Windows 64 bits (archivo python-3.8.10-amd64.exe)
	- Realizamos una instalación customizada (Install for all users - Create shortcuts for installed applications - Add Python to environment variables - Precompile standard library - pip - C:\\Program Files\\Python38) 
	- Verificamos la instalación de Python ejecutando desde consola ``python --version``
	- Verificamos la instalación de PIP ejecutando desde consola ``python -m pip --version``

:Sublime Text:
	- Herramienta de edición de texto y código fuente. Descargar `Sublime Text <https://www.sublimetext.com>`_



Creación de entorno virtual
===========================

.. code-block:: bash 

	# Lo siguiente se ejecuta desde consola, por ejemplo en C:\Users\Usuario>

	pip install virtualenv==20.4.6  # Instala la herramienta para generar entornos virtuales

	pip freeze  # Muestra el listado de paquetes instalados

	# Para crear un entorno virtual, creamos una carpeta donde se estarán todos los entornos virtuales.
	# Creamos por ejemplo la carpeta -> C:\Cosas\PIII\EntornosVirtuales

	cd C:\Cosas\PIII\EntornosVirtuales  # Accedemos a la carpeta

	virtualenv entorno01  # Creamos un entorno virtual llamado entorno01

	.\entorno01\Scripts\activate  # Activamos el entorno virtual

	# El comando anterior nos lleva al entorno virtual -> (entorno01) C:\Cosas\PIII\EntornosVirtuales>
	# También podemos ver que se creó un directorio nuevo -> C:\Cosas\PIII\EntornosVirtuales\entorno01 

	pip freeze  # Ejecutamos esto dentro del entorno virtual para ver los paquetes instalados

	pip install numpy  # Instalamos numpy
	pip install matplotlib  # Instalamos matplotlib
	pip install numpy==1.19.5  # Instalamos numpy en su versión 1.19.5

	deactivate  # Desactivamos el entorno virtual 
	
	# Para borrar el entorno virtual hay que borrar la carpeta donde se creó -> C:\Cosas\PIII\EntornosVirtuales\entorno01 

	# Si desea actualizar la versión de la herramienta pip, ejecutar lo siguiente:
	python.exe -m pip install --upgrade pip


Ejemplo en Google Colab
=======================


- `https://colab.research.google.com/drive/1z9ZRfwJ-9vCofQGCFNQtP5rT38dq0Dqg?usp=sharing <https://colab.research.google.com/drive/1z9ZRfwJ-9vCofQGCFNQtP5rT38dq0Dqg?usp=sharing>`_





Guía de ejercicios prácticos
============================

A continuación se propone una serie de actividades que cada estudiante debe seguir y luego documentar sus resultados en un archivo de texto dentro del repositorio.

1. **Verificar la instalación de Python y pip**

   - Abrir una ventana de terminal o consola.
   - Ejecutar los siguientes comandos para comprobar que Python y *pip* están correctamente instalados:

     .. code-block:: bash

        python --version
        python -m pip --version

   - Registrar las versiones obtenidas.

2. **Configurar un entorno virtual**

   - Crear una carpeta que agrupe los entornos de la materia, por ejemplo en ``C:\Cosas\PIII\EntornosVirtuales`` (Windows) o ``~/PIII/EntornosVirtuales`` (Linux/macOS).
   - Desde la consola, situarse en esa carpeta y crear un entorno llamado ``piii_env``:

     .. code-block:: bash

        cd C:\Cosas\PIII\EntornosVirtuales
        python -m venv piii_env

   - Activar el entorno virtual:

     * En Windows:

       .. code-block:: bash

          piii_env\Scripts\activate

     * En Linux/macOS:

       .. code-block:: bash

          source piii_env/bin/activate

   - Verificar que el prompt de la consola indica que estás dentro del entorno (suele aparecer ``(piii_env)`` al inicio de la línea).

3. **Instalar bibliotecas básicas**

   - Con el entorno virtual activado, instalar las bibliotecas ``numpy`` y ``matplotlib``:

     .. code-block:: bash

        pip install numpy matplotlib

   - Ejecutar ``pip freeze`` para listar los paquetes instalados y guardar este listado en un archivo ``requirements.txt`` usando:

     .. code-block:: bash

        pip freeze > requirements.txt

4. **Escribir y ejecutar un script de prueba**

   - Crear una carpeta para tus códigos (por ejemplo ``C:\Cosas\PIII\Codigos\clase01``).
   - Crear un archivo ``prueba_entorno.py`` con el siguiente contenido:

     .. code-block:: python

        import numpy as np
        import matplotlib.pyplot as plt

        x = np.linspace(0, 2*np.pi, 1000)
        y = np.sin(x)

        print("La suma de las primeras diez muestras es:", np.sum(y[:10]))

        plt.plot(x, y)
        plt.title("Seno de prueba")
        plt.xlabel("x [rad]")
        plt.ylabel("sin(x)")
        plt.grid(True)
        plt.show()

   - Guardar el archivo y ejecutarlo desde la consola dentro del entorno virtual mediante:

     .. code-block:: bash

        python C:\Cosas\PIII\Codigos\clase01\prueba_entorno.py

   - Comprobar que se muestra la gráfica y que se imprime el mensaje en la consola.

5. **Primeros pasos con Git y GitHub**

   - Crear una cuenta en `GitHub <https://github.com/join>`_ si aún no la tienes.
   - Crear un nuevo repositorio privado o público llamado ``curso-piii``.
   - Inicializar Git en la carpeta de códigos y realizar las primeras confirmaciones:

     .. code-block:: bash

        cd C:\Cosas\PIII\Codigos\clase01
        git init
        git config user.name "Tu nombre"
        git config user.email "tu_correo@example.com"
        git add prueba_entorno.py requirements.txt
        git commit -m "Primer script de prueba y listado de requisitos"

   - Conectar el repositorio local con el remoto y subir los cambios:

     .. code-block:: bash

        git remote add origin https://github.com/tu_usuario/curso-piii.git
        git branch -M main
        git push -u origin main

   - Verificar en la página de GitHub que los archivos ``prueba_entorno.py`` y ``requirements.txt`` están correctamente subidos.

6. **Documentar tus pasos**

   - Crear un archivo ``README.md`` en el repositorio donde describas los pasos que seguiste para instalar Python, crear el entorno virtual, ejecutar el script y subirlo a GitHub.
   - Añadir capturas de pantalla o comandos utilizados. Realiza un nuevo commit y *push* para actualizar el repositorio.

Estas actividades consolidan el uso de herramientas básicas que serán necesarias durante todo el curso. No dudes en pedir ayuda al docente o utilizar ChatGPT o similar para resolver problemas que surjan durante la instalación o configuración.
