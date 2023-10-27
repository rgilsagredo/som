# Hardware
La parte física del SI

## CPU
```
CPU = Central Processing Unit
```

Traducido a unidad central de procesos (a veces se dice de control de procesos).
También se conce como procesador.

Controla y ejecuta las operaciones del ordenador.

Tiene 2 partes

### CU
```
CU = Control Unit
```
Unidad de control, se encarga de dirigir operaciones. Para eso, tiene registros:
- registro de instrucciones: almacena la instrucción que se está ejeutando
- registro-contador de programas: tiene la dirección de memoria donde está la 
siguiente instrucción a ejecutar
- Controlador y decodificador: interpreta la instrucción
- secuenciador: coge una instrucción "grande" y la transforma en varias 
instrucciones "pequeñitas" (en secuencia)
- reloj: proporciona sucesión de impulsos eléctricos regularmente

Ejemplo: si le pides al ordenador que suem 2+2, la CU pilla esa instrucción,
la decodifica para saber qué quieres (quieres sumar), y se la pasa a la ALU
para que haga su trabajo

### ALU
```
ALU = Arithmetic Logic Unit
```
Unidad aritmático lógica; es la parte de la CPU que "hace las mates"
(sumas, restas, multiplicaciones, divisiones, lógica).

Operaciones hay de 2 tipos (más allá de suma etc):
- comparación: mayor/mayor o igual/menor/menoro igual/igual/distinto
- lógica: and/or/not/xor

En las operaciones de arriba entran datos y lo que sale siempre es un resultado
true/false.

Por ejemplo:
```
5 < 6 --> true
44 <= 9 --> false
```

En las operaciones de lógica, lo que entran son true/false y lo que sale es
un true/false (recordar que al final del día el ordenador trabaja con on/off
de impulsos eléctricos, lo que se traduce a lógica como true/false o 1/0)

Por ejemplo:
```
(4 <= 5) AND (4 + 4 = 8) --> TRUE
```

Operaciones lógicas:
- AND: también se suele representar con `&`, y para devolver T es encesario
que ambos operandos sean T
- OR: también se suele representar con `|` y para devolver T al menos un
operando debe ser T
- NOT: se suele representar con `!` y cambia T por F
- XOR: también conocido ocmo eXclusive OR, para devolver T los operandos deben
ser distintos

### Ejercicios
Hacer tablas de verdad, inventarse algunas operaciones y combinarlas
para ver cómo funcionan las operaciones de la ALU

### Características de un (micro)procesador
#### Cores
O núcleos. Cada core es un procesador en si mismo; cooperan entre si.
A más cores, mas capaciad de procesamiento (más cosas a la vez puede hacer)

#### Frecuencia del reloj
Se mide en Hz (Herzios). Mide el nº de millones de veces por segundo que el
procesador es capaz de conmutar corriente (conmutar es pasar electricidad
de un circuito a otro). Esto determina la rapidez con la que el procesador
hace sus cosas

#### Transitores
Es una componente que permite o no el paso de electricidad (0/1). 
Se mide en nanómetros. Cuanto más pequeños, menos calenmtamiento, más 
rendimiento

#### Tamaño manipulación de datos
32 o 64 bits. Determina cuanta memoria puedo usar

### Familias de arquitecturas
Hay 2 grandes, `Reduced Instruction Set Computer = RISC`; son procesadores con
un conjunto simple de instrucciones. Ejemplos: ARM, PowerPC.

La otra es `Complex Instruction Set Computer = CISC`; porcesadores
con instrucciones más complejas. Ejemplos: x86 (32 bits) y x86_64 (64 bits)

## Memoria
Se encarga de almancenar software y datos que usa el SI

### Memoria de almacenamiento externo
Es permanente, está en periféricos de almacenamiento (HD o PenDrives)

### Memoria interna ROM
```
ROM = Read Only Memory
```
Es una memoria solo de lectura (no puedo modificar lo que hay ahí) que 
permite el arranque. Realemnte sí que es modificable a veces para dar
actualizaciones. Es una memoria no volátil (ie, si apago el ordenador
no desaparece). En la placa base de los ordenadores tenemos la
`BIOS = Basic Input Output System`, que es un programa almancenado en una ROM
y el que se encarga de lanzar el OS.

### Memoria RAM
```
RAM = Random Access Memory
```
Almacena de manera temporal (volátil) datos y programas. El CPU lee de aquí.
Características:
- volatil: si se apaga el dispositivo, no almacena nada
- rápida: más rápido que memoria de almacenamiento externa
- cara: más que la de almacenamiento externa

Se constituye de *cells* que almacenan *1 byte = 8 bits* 
(un bit = 0 o 1, T/F, ON/OFF). Osea, que cada cell alamacena info en binario
(explicar lo que es binario??). Cada cell tiene una dirección de memoria,
para que el procesador la encuentre y modifique.

El tamaño que use el procesador para referirse a estas direcciones de memoria
condiciona el máximo de memoria que puede usarse:

- Un porcesador de 32 bits puede acceder a 2^32 celdas distintas (la celda
00...00, la celda 00...01, la celda 00...10, 00..11,...; hay 2^32 
posibilidades). Y eso son 4GB de memoria.

- Un procesador de 64 no hay limitación práctica de memoria (2^64 es un
número muy grande)

La RAM actual se denomina DDR (Double Data Rate), va por la g5

## Buses
Son lineas eléctricas u ópticas pata que exista comuniación entre CPU, memoria
y resto de cosas. Tipos:
- Bus de datos: intercambio de datos entre CPU y dispositivos
- Bus de direcciones: comunica dirección de memoria o dispositivo con el que se
cambia info
- Bus de control: Para que la CPU transmita a dispositivos orden a ejecutar
y se devuelva resultado

También puedes distinguir entre buses internos (conectan componentes internas)
y buses de expansión (concectan CPU con periféricos).

Por el bus, la info puede ir en serie (bit a bit, uno tras otro), o en
paralelo (varias lineas, por cada cual va un bit). La teoría es que los 
paralelos son más rápidos, pero necesitas sincronización. Los buses de expansión
actuales suelen ser en serie.

- Buses en serie: PCI-Express, USB, SATA
- Buses en paralelo: AGP, IDE, PCI (obsoletos)

## I/O
```
I/O = Input/OutPut
``` 
También periféricos o dispositivos de E/S. Permiten comunicarse con el SI

### Input
Permiten entrada de info al ordenarod: tecaldo, ratón

### Output
Proporcionan info generada por el ordenador: pantalla, impresora

### I/O
Hacen las 2 anteriores a la vez. Por ejemplo, tarjeta de red.

### Storage
De almacenamiento. Discos duros o pendrives.


### Software de devices
Los I/O Devices requieren software para su uso.
Si el software está en el propio periférico, se llama firmware.
Si está en el ordenador (lo ejecuta la CPU) se llama driver

