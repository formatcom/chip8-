- chip8

- memory map
------------------------ 0xFFF
| display memory (ram) |
|                      |
------------------------ 0xF00
|                      |
|                      |
|                      |
|   free memory ram    |
|                      |
|                      |
|                      |
------------------------ 0x200
|   interpreter (rom)  |
|   font data          |
------------------------ 0x000

- registers

 el chip8 tiene 16 registros de proposito
 general, comunmente llamado Vx, donde x 
 es un digito hexadecimal.

   V0 ... VF cada uno de 8 bit

 el registro VF funciona como un flag o 
 Registro de Estado (Status Register). Se 
 usa como carry flag (cuando se usan
 instrucciones aritméticas) o como
 detector de colisiones (cuando se
 dibujan Sprites).

 el registro de direccionamiento I de 16 bit,
 se utiliza por algunas instrucciones que
 acceden a la memoria.

   I registro de direccionamiento.

 existen otros dos registros especiales de
 8 bit que se utilizan por los dos timer
 del chip8. Si tienen un valor diferente de
 0, se decrementan una unidad cada 60 hz.

   DT (delay timer) para hacer retardos.
   ST (sound timer) produce sonido hasta
  llegar a 0.

   PC (contador del programa) 16 bit
   SP (puntero de pila) 8 bit


- opcode 36 instrucciones

 todas las instrucciones son de 2 bytes (16 bit)
 nomeclatura:

    nnn       direccion        12 bit
    kk        numero            8 bit
    n         numero            4 bit
    x         registro Vx       4 bit
    y         registro Vy       4 bit

------------------------------------------
          instrucciones de carga
------------------------------------------
6xkk       LD Vx, kk

  guarda el valor kk en el regitro Vx.

8xy0       LD Vx, Vy

  guarda el valor del registro Vy en el
  registro Vx.

Fx07       LD Vx, DT

  guarda en Vx el valor de DT (delay time).

Fx15       LD DT, Vx

  guarda en DT (delay time) el valor de Vx.

Fx18       LD ST, Vx

  guarda en ST (sound time) el valor de Vx.

Annn       LD I, nnn

  guarda en I el valor de nnn ( 12 bit ).

Fx29       LD F, Vx

  esta instruccion es un tanto especial asi
  que me la explico con calma.

  primero definamos las caracteristicas de
  un sprite para el chip8.

       - sprite
                 ancho 8 px
                 alto de 1 a 8 px

       - ejemplo sprite [0] 8x5 (5 bytes)

decimal       binario       hexadecimal

0             11110000      0xF0
              10010000      0x90
              10010000      0x90
              10010000      0x90
              11110000      0xF0

  con esto claro debemos saber donde estan
  guardado en memoria estos sprite.

  sprite font direccion 000h a 200h que son
  los numeros del 0 al F, cada sprite de 8x5
  es decir 5 bytes como en el ejemplo.

  el valor del registro Vx es un digito del
  0 - F. (ya que solo soportaremos esos 
  digitos)

  la instruccion es asignar al registro I la
  direccion de memoria que apunta al sprite
  que corresponde al valor de Vx.

    - ejemplo para Vx = 2

  en la direccion de memoria 000h tenemos el
  sprite 0, como sabemos que el sprite ocupa
  5 bytes en la direccion 005h tenemos el 
  sprite 1, entonces el sprite 2 esta en la
  direccion 00Ah.

  entonces podemos decir que:

     I = Vx * 5
  
Fx33       LD B, Vx
