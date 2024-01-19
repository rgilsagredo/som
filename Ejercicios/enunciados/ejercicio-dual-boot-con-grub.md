# Idea
Vamos a crear una VM que tenga un dual boot W7 y Ubuntu20.04 con arranque en 
BIOS, particionado DOS y GRUB como BootManager

## Paso 1: particionado de disco
En vez de ir "a lo bruto" e instalar en un disco virgen, primero pensamos
qué queremos y luego hacemos las instalaciones.

Digamos que vamos a tener un único disco duro de 500GiB en la máquina,
con 4GiB de RAM.

Investigando un poco los requisitos hardware:

https://support.microsoft.com/en-us/windows/windows-7-system-requirements-df0900f2-3513-a851-13e7-0d50bc24e15f

vemos que Windows 7 necesita 2GiB de RAM y 20GiB de disco para su versión 
de 64 bits y que Ubuntu20.04:

https://docs.cpanel.net/installation-guide/system-requirements-ubuntu/

necesita 1GiB de RAM y 20GiB de disco (minimo).

Con lo que tenemos debería de sobra darnos para ambos OSs.

Como en principio no hay ninguna otra restricción, podemos asumir que
partimos el disco en 2, y la mitad para cada OS. Como el arranque es BIOS,
la única manera de tener 2 OS en un único disco y poder seleccionar
cual arranca es tener un Bootmanager; vamos a instalar GRUB como
Bootmanager, lo cual implica que también necesitaremos una partición
para almacenar `/boot/grub` (los otros 2 ficheros que necesita GRUB van,
uno en el MBR y el otro en el espacio post MBR). Esa partición basta que sea
de 32MiB y su tipo ext4 (en principio admite otros FS).

Además, por hacer las cosas bien, por cada OS que instalemos vamos a
crear una partición para el sistema (los programas) y otra para los datos.
Y además pensamos en que quizás en un futuro necesitemos otras particiones
para cualquiera de los 2 OS, así que vamos a dejarnos espacio.

Con todo esto, un posible particionado sería:
- una particón primaria de 32MiB para `/boot/grub`
- una partición extendida que ocupe el resto del diso (a efectos prácticos 
    500GiB)
- una partición lógica de 50GiB para el sistema de Windows7
- Una partición lógica de 100GiB para los datos de Windows7
- Una partición lógica de 50GiB para el sistema de Ubuntu
- Una partición lógica de 100GiB para los datos de Ubuntu

Así de los 500GiB totales del disco, 250 van para cada OS, de los
cuales usamos 150GiB para cada, y les dejamos 50GiB libreas a cada uno
por si necesitásemos algo extra en el futuro.

Ahora que sabemos lo que queremos, vamos a hacer el particionado
incluso antes de tener el disco, y luego copiaremos ese particionado 
al disco y haremos las instalaciones.

Creamos un fichero vacío que "haga de disco", y usando la herramienta que te de
la gana (yo voy a usar sfdisk) creamos el particionado:



