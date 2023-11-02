# Datos
Son la materia prima con la que operan los SI

## Representación de la información
EL ordenador al final del día (el hardware) fuciona con impulsos electricidad,
y puede haber o no electricidad. Es decir, fiunciona en si/no T7F, 0/1.

Eso se denomina sistema binario (binario = 2 dígitos).

### Sistemas de numeración
Es un conjunto de símbolos y reglas que permiten representar cantidades.
El que utiliozamos en el dia a dia (el decimal) se llama posicional
porque las cantidades se expresan mediante unos dígitos, y la posicion de
esos dígitos importa. Hay tantos dígitos como lo que se lama "la base" del
sistema. El 0 y cosas menores que la base se representan con los dígitos,
de la base en adelante ya son combos de dígitos (cuya posición importa;
no es lo mismo 19 que 91)

#### Decimal
La base es 10, porque hay 10 dígitos (o porque hay 10 dígitos la base es 10),
que son `0 1 2 3 4 5 6 7 8 9`.

#### binario
La base es 2, por tanto tiene 2 dítigos: `0 1`. Realmente funciona igual que el
decimal, solo que al ser la base menor, necesitas más dígitos para reproducir
númeors más grandes.  a la izda decimal y a la dcha bin

- 0 = 0
- 1 = 1
- 2 = 10
- 3 = 11
- 4 = 100
- 5 = 101
- 6 = 110
- 7 = 111
- 8 = 1000
- etc

(chiste de hay 10 tipos de personas)

Las reglas aritméticas (sumas, restas, multiplicaciones, divisiones)
son igual que en el decimal; solo que hay 2 dígitos:

```
3+4 = 011 + 100 = 111 = 7
7+5 = 111 + 101 = 1100 = 12
```

##### ejercicio?
hacer sumas y restas y multiplicaciones y divisiones de enteros en binario

#### hexadecimal
su base es 16, así que representa cantidades con 16 dígitos:
`0 1 2 3 4 5 6 7 8 9 A B C D E F`. Se usa mucho porque permite representyar
cantidades grandes con ménos dígitos (a base más grande, menos dígitos para
representar grnades cantidaes), es cercano al decimal para que un humanos lo p
ueda pillar rápido, y es fácil de transformar a binario (el de los ordenadores)
porque 16=2^4.

De nuevo, funciona igual que el decimal:
```
0, 1, 2, 3, 4, 5, 6, ,7 ,8, ,9, A, B, C, D ,E, F, 10, 11, ... 19, 1A, 1B, 1C... 
1F, 20, 21, ... FF, 100, 101... 
```

idem operaciones aritmáticas (pero aquí es u poco mas difícil
porque tendríamos que sabernos las tablas de sumar y multiplicar)

#### Octal
De momento pasando. Es el mismo rollo, base 8

#### Nomenclatura
Es común que se use la siguiente nomenclatura para decir en qué base estoy
representando un número:
- prefijo **0b**: es un número expresdado en binario: *0b110101011*
- prefijo **0**: es un número expresado en octal: *03471*
- prefijo **0x**: es un número expresado en hexadecimal: *0xFD5A*
- sin prefijo: expresado en decimal: *1234*

#### conversiones

##### Lo que sea a decimal
Me da igual la base (me la tienen que decir), pero todo es fácil de convertir
a decimal, basta hacer un cálñculo usando la base y las posiciones.
Si me dan 1001 en binario, este número se entiende como:
```
1*2^3+0*2^20*2^1+1*2^0=1*8+1=9
```
igual es hexa:
```
4C = 4*16^1+12*16^0=76
``` 

###### Ejercicio
hacer algunas conversiones mas a decimal para pillar soltura

##### decimal a binario/hexa/lo que sea
Hay que ir dividiendo por la base, y guardando los restos y la parte entera.

###### como dvidir (en decimal)
quieres calcular parte entera y resto de a/b. Se asume que a >= b; si no
parte entera = 0 y parte decimal = a. 

Como a>b, a tendrá las mismas cifras o más que b. Buscamos el primer grupo
de cifras que sea >b. Probamos con múltiplos de b hasta que nos pasemos.
El anterior es el que se usa, y se resta. Continuamos hasta que qude algo < b

Ejemplo: dividir 567/18:
5 no es mayor que 18. 56 sí es mayor que 18. tras probar, vemos que
18*4 > 56 y 18 *3= 54 <= 56. Así que la primera cifra es 3. Restas
54 a 56 y bajas lo que queda del numero original (el 7). Entonces repites el
proceso ahora con 27/18; te sale que la siguiente cifra es 1 y el resto 9 <18.
Así que la division queda 567 = 18*31+9; siendo el 31 la parte entera y el 9 e 
resto.

Esto es lo que se usa para pasar de decimal a binario. En caso de binario,
hay que ir dividiendo por 2, y guardamos restos y partes enteras. los
restos me dan el numero en binario al revés, 
las partes enteras, mientras no sean 0,
me dicen que hay que seguir dividiendo.

Ejemplo: expresar 10 en binario: (hago divisiones) y saco que
- 10/2 --> 10 = 2*5+0
- 5/2 --> 5 = 2*2+1
- 2/2 --> 2 = 2*1+0
- 1/2 --> 1 = 2*0+1

Cuando tenemos la parte entera 0, los restos forman el binario 10 = 1010

Igual para pasar a hexa: expresar 123 en hexa:
- 123/16 --> 123 = 16*7+11
- 7/16 --> 7 = 16*0+7

por tanto: 123 = 7B

###### Ejercicio
hacer más de esto si se necesita solñtura

##### de hexa a binario y viceversa
como 16=2^4, resulta que podemos aprendernos la expresión en binario
de todos los dígitos hexa, y eso vale para pasar a binario.
Se agrupan los binarios de 4 en 4
(izda hexa dcha bin)
- 0 = 0000
- 1 = 0001
- 2 = 0010
- 3 = 0011
- 4 = 0100
- 5 = 0101
- 6 = 0110
- 7 = 0111
- 8 = 1000
- 9 = 1001
- A = 1010
- B = 1011
- C = 1100
- D = 1101
- E = 1110
- F = 1111

Y con esto, si tegno F2B en haxa, su decimal es sustitur cifra a cifra, es decir
F2B = (1111)(0010)(1011)

La conversión a la inversa también funciona, agrupando el binario de 4 en 4 
(empezar a dcha, si en el último faltan cifras, son ceros)

1010010100101 = (0001)(0100)(1010)(0101) = 14A5

##### Ejercicios
convertir a bin y hexa: 43 67 95 121 193 217 675
inventase cosqas para mas conversiones

##### Cosas
la calculadora de windows, la pones en modo programador, y te hace
lñas conversiones soals

### Tipo de datos
Aunque el ordenador guarde btodo en formato binario, los datos de la vida
vienen en distintos formatos; existen por tanto distintos mecanismos para
transformarlos a binario.

En general hay 3 tipos de datos:
- números enteros (sin decimales)
- números reales (con decimales)
- caracteres (letras)

Hay que decidir también cuánto espacio de memoria vamos a darle a cada dato que
queramos almacenar, es decir, cuantos bytes le reservamos. Por ejemplo, puedes
decidir que para representar caracteres vas a usar un byte, que me da 2^8
opciones. Quizás esto sea suficiente para los caracteeres de nuestro idioma,
pero no para los de por ejemplo el chino. Ídem con los números, si damos
por ejemplo 2 bytes para representar números, eso son 2^16 posibles números
a guardar, es fácil darse cuenta de qu eso no son muchos números.

Cuanta más memoria dejemos, má grande es lo que podemos almacenar (obvio)

#### Representación de enteros
Los enteros son 0, &plusmn; 1, &plusmn; 2, ...
El problema obviamente es que hay que representar los negativos; si no,
la conversión es directa. Hay varias estrategias, la primera es la más usada:

##### Complemento a 2
Primero haces el complemento a 1, y luego le sumas 1, y así tienes la
representación del negativo:

```
3 = 0b00000011
-3 = 0b11111100 + 0b00000001 = 0b11111101
```


##### Complemento a 1
Se reserva el primer bit para que 0=positivo, y el negativo de un número es
cambiar 0s por 1s:
```
9  = 00001001
-9 = 11110110 
```
Problemas: hay 2 ceros, la resta puede dar números de 9 cifras, y solo puedo
tener 8 para representar. Si pasa eso, hay que llevarse el 1 de delante, 
"sacarlo" y sumarlo a los otros 8 bits:

```
50 = 0b00110010
-8 = 0b11110111
50 + (-8) = 0b00110010 + 0b11110111 = 0b100101001
```

Ese 1 del principio "se saca" y "se suma":
```
0b100101001 --> 0b00000001 + 0b00101001 = 0b00101010 = 42
```

##### Bit de signo
Se reserva el primer bit del byte para signo: 0 pòsitivo, 1 negativo.
entonces `9=00001001` y `-9=10001001`. Presenta problemas: nos quita un bit
(menos espacio para almacenar números), suma y resta tienen tratamientos 
diferentes, hay 2 ceros

##### Mover el cero
Con 8 bits puedo representar hasta el número 255. Mover el cero es que
represento números solo positivos, pero "el del medio" (127) es mi nuevo
cero. Como cualquier número es `7 = 7-0`, solo tengo que poner "mi nuevo
cero" para obtener los negativos

#### Representación de reales
Los reales son números enteros a los que "les cuelga" la parte decimal
(también llamada mantisa): `123'456`. Entonces, de aquí se desarrolla la 
notación de *coma flotante normalizada*, que consite en representar al número
como un único número entero, la parte decimal, y una potencia de 10:
```
123'456 = 1'23456 * 10^2
```
(el número de movimientos de la coma es el exponente que necesito)
Entonces con esto puedo representar números con decimales usando 3 cosas:
- 0/1 para signo
- lo que es el núemro en sí
- el exponente
```
123'456 = (0, 123456, 2) = (signo, numero, exponente)
```
(notar que la coma siempre va en el mismo sitio)

Puedes hacer exactamente lo mismo en binario:
```
100110.00101 = (0, 10011000101, 5) = (0, 10011000101, 101)
```

Lo que hace el ordenador es reservar espacio para cada una de estas 3 
componentes del número: 1 bit para signo siempre, y, según queramos más o menos 
precisión:
- precision simple: 8 bits para exponente, 23 para número. Total: 32 bits
- precisión doble: 11 bits para exponente, 52 para número. Total: 56 bits

Pero en realidad, lo que hay a la izda de la coma siempre va a ser un 1; así
que no se representa; solo pones lo que está a dcha de la coma. En el ejemplo
de arriba:

```
100110.00101 = (0, 0011000101, 5) = (0, 0011000101, 101)
```
y quedaría pendiente rellenar con ceros a derecha el número hasta completar
espacio, y a izda el exponente hasta completar espacio.

Si el exponente es negativo, se usa el complemento a 2 para representarlo
(por ejemplo en el número 0.010101).

- El exponente te habla de la magnitud: más exponente es un númeor más grande
- El número te dice la precisión: más bist, mas preciso

#### Representación de caracteres
Para representar caracteres alfanuméricos (letras, símbolos de puntuación
o de control (como espacio en blanco o salto de linea)) se crean tablas
(diccionarios) de condificación que traducen cada caracter alfanumérico en
un número binario.

Se llama *sistema ed codificación* a un "diccionario" que traduce ciertos
caracteres a binario (por qué varios: principalmente por los idiomas;
no es lo mismo codificar los símbolos que se tienen en castellano que 
los que se tienen en por ejemplo chino, que son muchos más)

##### DE ancho fijo
Son sistemas de codificación donde los números binarios todos van a tener
la misma longitud

###### ASCII
```
ASCII = American Standard Code for Information Interchange
```
Sistema de 7 bits, representa carateres de control , puntuación y alfabeto
inglés. La gran mayoría de los sistemas de condificacion son compatibles con 
ASCII. Ver https://ascii.cl/es/. Como es un sistema de 7 bits, codifica 2^7
caracteres (no son muchos)

La info suele agruparse en bytes (8 bits); hay sistemas de codificación
que se llaman ASCII extendido que son de ancho fijo 8 bits = 1 byte, 
y que los primeros 128 = 2^7 caracteres son los mismo que el ASCII
(ie son ASCII + más cosas). Los más usados son 
```
CP147
CP850
CP1252 Windows-1252
ISO/IEX 8859
```
Quizás merece la pena mencionar el ISO-8859 un poco más; en realidad son
15 sistemas de condificción, que extienden ASCII, cada cual orientado a 
un conjunto de lenguas. ISO-8859-1 se ve con frecuencia porque es el que 
codifica languas de europa occidental.

###### Ejemplo
Jugar con un notepad++ para ver lo de las codificaciónes. Escribir un texto
que tenga acentos, sómbolos raros, etc. Guardar. Pinchar en "convert to ANSI",
escoger oro charset, ver cómo cambia


###### UTF-32
```
UTF = Unicode Transformation Format
```
DE ancho fijo de 32 bits (2^32 símbolo puede condificar, eso son ya bastantes)
Tiene 2 CONS: ocupa mucho y no es compatible con ASCII

##### Ancho variable
No todos los códigos tienen la misma longitud (obvio).

###### UTF-16
Usa 2 o 4 bytes para codificar todos los caracteres. Los caractreres más
comunes van en la parte de 2 bytes para optimizar tamaño. No es comparible con 
ASCII

###### UTF-8 
Usa de 1 a 4 bytes. Los de 1 byte son los ASCII, así que es compatible.
Con 2 bytes usa caracteres para lenguas europeas, árabe, hebreo.
3 bytes para caracteres no linguísticos de uso común, y chino/japonés,
4 bytes para el resto. En general este es el encoding que se usa. CONS: es 
lento.

VER: https://www.utf8-chartable.de


## Unidades de la información

### Cantidad
Para almacenar cantidades, lo mínimo que se usa es el bit (0/1).
Como en ordenadores las cosas van en binario, lo normal es que los
"pasos a mayores" (como de metros a kms) sea no potencias de 10, si
no potencias de 2 (en concreto, que aumentas 2^10=1024 veces la cantidad).

Entonces se usan 2 notaciones distintas: si quiero decir que aumento 
una cantidad en multiplos de 10^3=1000, lo escribes "normal", si 
quiero decir que aumento en 2^10, escribes una "i". Ejemplo:
```
1 MB (Mega Bytes) = 1000 KB (1000 Kilo Byte)
1 MiB (Mega Bytes) = 1024 KB (1024 Kilo Byte)
```

La tabla para representar cantidades es:
```
b = bit
B = byte = 8 bits
KB = Kilo Byte =  1000 B
MB = Mega Byte = 1000 KB
GB = Giga Byte = 1000 MB
TB = Tera Byte = 1000 GB
PB = Peta Byte = 1000 TB    
```

Hay más pero con esto vives muy bien. La tabla de conversiones "con la i" es
igual pero cmabias 1000  por 1024

#### Ejercicio
Hacer alguna conversión de cantidades para sentirse cómodo
con las unidades

### Velocidad
Para medir lo rápido que se pasa la información. Interesa saber cuanta
info se envia/recibe *por unidad de tiempo* (es decir, por segundo)

#### TRansferencia en red
Se usa el bit como unidad base, y siempre multiplos de 1000:
```
b/s = bits por segundo
Kb/s = Kilobits por segundo = 1000 b/s
Mb/s = Megabits por segundo = 1000 Kb/s
Gb/s = Gigabits por segundo = 1000 Mb/s
etc
```

#### Transferencia entre dispositivos
Por ejemplo a un USB. Aquí se habla en en bytes y múltiplos de 1024
```
B/s = Bytes por segundo
KiB/s = KiloBytes por segundo = 1024 B/s
MiB/s = MegaBytes por segundo = 1024 KiB/s
etc
```

Más o menos (seguramente no esté actualizado) una idea de órdenes de magnitud
de transferencia:

![magnitudes-transferencia](./images/magnitudes-transferencia.png "Magnitudes de transferencia")

##### Ejercicios
- Convertir:
    - 2 millones de b/s a Mb/s.
    - 15GB a KB.
    - 1TiB a MiB
    - 51200b a KB
    - 200Mb/s a b/s

- He comprado un disco marcado como de 2TB. ¿Cuántos GiB nos dirá el sistema 
operativo que tiene?

- Si disponemos de dos conexiones a internet de 512Kb/s y 2Mb/s, ¿cuál será 
nuestro ancho total de salida a internet si configuramos apropiadamente el 
sistema para que balancee la carga entre ambas conexiones?