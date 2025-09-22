.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 10 - PIII 2025
====================
(Fecha: 22 de septiembre)



NRZ (Non-Return-to-Zero)
------------------------

- Es un tipo de codificación digital utilizada para transmitir datos binarios en telecomunicaciones y sistemas digitales. 
- Los bits se representan por dos niveles de voltaje sin retornar a cero después de cada símbolo.
- Nota de niveles: en lógica TTL/CMOS suele usarse 3.3 V/0 V o 5 V/0 V (bajo ≈ 0 V); en enlaces diferenciales (p. ej. Ethernet) los niveles típicos son de baja amplitud y centrados en 0 V (≈ ±1 V diferenciales).


NRZ-L (Non-Return-to-Zero-Level)
--------------------------------

- Cada bit se representa directamente con un nivel de voltaje fijo:

  - '1' → nivel de voltaje alto.
  - '0' → nivel de voltaje bajo (cercano a 0 V).

**Ejemplo (bits: 1 0 0 1 1 0)**  
(El nivel se mantiene durante todo el bit; solo cambia al cruzar de un bit a otro)

.. code-block:: text

  tiempo →  | b1 | b2 | b3 | b4 | b5 | b6 |
  bits      |  1 |  0 |  0 |  1 |  1 |  0 |
  NRZ-L     | ‾‾‾| ___| ___| ‾‾‾| ‾‾‾| ___|


NRZ-I (Non-Return-to-Zero-Inverted)
-----------------------------------

- El valor '1' se representa mediante un **cambio** (inversión) del nivel.
- El valor '0' se representa como **ausencia de cambio** (se mantiene el nivel).
- Se asume un nivel inicial (alto o bajo); aquí tomamos **bajo** como arranque.

**Ejemplo visual (bits: 1 0 0 1 1 0, nivel inicial = bajo)**

.. code-block:: text

    tiempo →  | b1 | b2 | b3 | b4 | b5 | b6 |
    bits      |  1 |  0 |  0 |  1 |  1 |  0 |
    regla     |inv |mant|mant|inv |inv |mant|
    nivel ini: bajo
    
    NRZ-I     ___ ___ ___ ─── ___ ___
              ↑inv       ↓inv   ↑inv

   Paso a paso:
   - b1=1: invierte (bajo→alto) → queda alto
   - b2=0: mantiene → sigue alto
   - b3=0: mantiene → sigue alto
   - b4=1: invierte (alto→bajo) → queda bajo
   - b5=1: invierte (bajo→alto) → queda alto
   - b6=0: mantiene → sigue alto



Simulación de transmisión Ethernet usando NRZ-L
-----------------------------------------------

.. code-block:: python

   # Codificación NRZ para simular transmisión NRZ-L

   def string_to_bits(message: str) -> str:
       """Convierte texto a cadena de bits (8 bits por carácter)."""
       return ''.join(format(ord(c), '08b') for c in message)

   def nrz_encode(bits: str, v_high=1.0, v_low=-1.0):
       """
       Codifica la secuencia binaria usando NRZ-L (Level).
       - '1' -> v_high
       - '0' -> v_low
       """
       return [v_high if b == '1' else v_low for b in bits]

   def transmit_signal(levels):
       """“Transmite” imprimiendo los niveles (solo a modo ilustrativo)."""
       print(' '.join(f'{lvl:g}' for lvl in levels))

   # Ejemplo de uso
   message = "Hola"
   bits = string_to_bits(message)
   print(f"Mensaje: {message}")
   print(f"Bits:     {bits}")

   # Opción 1 (educativa, bipolar): +1 / -1
   nrz_levels = nrz_encode(bits, v_high=1.0, v_low=-1.0)

   # Opción 2 (TTL/CMOS): 3.3 V / 0 V  -> descomentar si querés esa visual
   # nrz_levels = nrz_encode(bits, v_high=3.3, v_low=0.0)

   print("Señal codificada (NRZ-L):")
   transmit_signal(nrz_levels)


Transmisión de Datos en 1000BASE-T (Gigabit Ethernet)
=====================================================

- **Medio**: cuatro pares UTP (Cat 5e o superior) en **full-dúplex**.
- **Señalización**: **4D-PAM5** (5 niveles: −2, −1, 0, +1, +2) en los **4 pares** simultáneamente.
- **Velocidad**: ~250 Mbaud por par → combinados alcanzan **1 Gbps**.
- **Notas**:
  - El nivel adicional (0) actúa como nivel central para balance DC y posibilita redundancia/recuperación de errores.
  - Para fines didácticos, abajo se usa un **mapeo simplificado** (2 bits → 4 niveles), reservando el **0**.


Simulación didáctica (PAM5 1D)
------------------------------

.. code-block:: python

    import numpy as np
    import matplotlib.pyplot as plt

    # ===== PAM5: niveles (5) =====
    PAM5_LEVELS = np.array([-2, -1, 0, 1, 2], dtype=float)

    # ===== Mapeo didáctico (2 bits -> 4 niveles; 0 queda reservado) =====
    # 00 -> -2, 01 -> -1, 10 -> +1, 11 -> +2
    ENC_MAP = {
        (0, 0): -2.0,
        (0, 1): -1.0,
        (1, 0): +1.0,
        (1, 1): +2.0,
    }
    DEC_MAP = {v: k for k, v in ENC_MAP.items()}  # inverso exacto

    def encode_pam5_2bits_per_symbol(bitstream):
        """Agrupa bits de a 2 y mapea a niveles PAM5 (usando 4 de los 5 niveles)."""
        if len(bitstream) % 2 != 0:
            raise ValueError("La longitud de bits debe ser par.")
        symbols = []
        for i in range(0, len(bitstream), 2):
            pair = (int(bitstream[i]), int(bitstream[i+1]))
            symbols.append(ENC_MAP[pair])
        return np.array(symbols, dtype=float)

    def quantize_to_pam5(x):
        """Cuantiza al vecino PAM5 más cercano (tolerante a ruido)."""
        idx = np.abs(PAM5_LEVELS.reshape(-1, 1) - x.reshape(1, -1)).argmin(axis=0)
        return PAM5_LEVELS[idx]

    def decode_pam5_2bits_per_symbol(symbols):
        """Decodifica asumiendo mapeo de 4 niveles; si aparece 0, lo marca como 'reserva'."""
        bits = []
        for s in symbols:
            s_q = PAM5_LEVELS[np.argmin(np.abs(PAM5_LEVELS - s))]  # cuantización
            if s_q == 0.0:
                # Nivel reservado en este esquema didáctico
                # Podrías decidir descartarlo o mapearlo a (0,0) con una advertencia.
                raise ValueError("Apareció nivel 0 (reservado) en esta codificación simplificada.")
            b0, b1 = DEC_MAP[s_q]
            bits.extend([b0, b1])
        return np.array(bits, dtype=int)

    # ===== Reparto 4D (ilustrativo) =====
    def split_to_4d_round_robin(symbols):
        """Distribuye símbolos a 4 pares A,B,C,D en round-robin (ilustración 4D)."""
        A = symbols[0::4]
        B = symbols[1::4]
        C = symbols[2::4]
        D = symbols[3::4]
        return A, B, C, D

    def merge_from_4d_round_robin(A, B, C, D):
        """Reconstruye la secuencia 1D desde 4 streams round-robin."""
        L = max(len(A), len(B), len(C), len(D))
        out = []
        for i in range(L):
            if i < len(A): out.append(A[i])
            if i < len(B): out.append(B[i])
            if i < len(C): out.append(C[i])
            if i < len(D): out.append(D[i])
        return np.array(out, dtype=float)

    # ===== Ejemplo =====
    original_bits = np.array([1,0, 1,1, 0,0, 1,0,  1,0, 0,1, 1,1, 0,0])  # 16 bits (8 símbolos)
    symbols_1d   = encode_pam5_2bits_per_symbol(original_bits)

    # Simular “ruido” leve en el dominio 1D (solo para mostrar la cuantización)
    noisy = symbols_1d + 0.1*np.random.randn(*symbols_1d.shape)

    # Cuantización y decodificación
    symbols_1d_q = quantize_to_pam5(noisy)
    decoded_bits = decode_pam5_2bits_per_symbol(symbols_1d_q)

    print("Bits originales:      ", original_bits)
    print("Símbolos PAM5 (1D):   ", symbols_1d)
    print("Símbolos + ruido:     ", noisy.round(2))
    print("Cuantizados a PAM5:   ", symbols_1d_q)
    print("Bits decodificados:   ", decoded_bits)

    # Ilustración 4D (reparto a 4 pares)
    A, B, C, D = split_to_4d_round_robin(symbols_1d)
    print("\nStreams por par (4D, round-robin):")
    print("Par A:", A)
    print("Par B:", B)
    print("Par C:", C)
    print("Par D:", D)

    # Gráfica simple de los símbolos 1D
    plt.figure(figsize=(6,3))
    markerline, stemlines, baseline = plt.stem(range(len(symbols_1d)), symbols_1d)
    plt.title("Símbolos PAM5 (1D) – mapeo didáctico 2 bits/ símbolo")
    plt.xlabel("Índice de símbolo")
    plt.ylabel("Amplitud")
    plt.grid(True)
    plt.show()


Ejercicio 17:
=============

- Simular la transmisión de un mensaje entre dos computadoras.  
- Elegir un protocolo de la pila **TCP/IP**.  
- Utilizar Gigabit Ethernet sobre UTP con **4D-PAM5**.  
- Incluir el uso de un **filtro sinc con caída cosenoidal**.  




----

--------------------------
Entrega en GitHub
--------------------------
1. Subir códigos, ipynb, gráficos generados, etc.
2. Usar la carpeta Clase10
3. Documentar en ``README.md`` el ejercicio.



