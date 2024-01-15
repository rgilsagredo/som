# Ejercicio particionado GPT
Se usa la herramienta ``sgdisk``, muy parecida a ``sfdisk`` en cuanto
a filosofía pero se usa distinta.

Primero, crear un disco qu se llame ``0.disk`` de 100G en la carpeta 
`/tmp/`

Para consultar tabla de particiones, usamos el comando

Para ver qué podemos hacer con `sgdisk` lanzamos el comando

```
sgdisk --help
```

Para consultar tabla de particiones hacemos:

```
sgdisk -p 0.disk
```

Como no hemos creado aun nada, debería decirnos esto:

![tabla-particiones-vacia](./images-ejercicio-particiones-GPT/tabla-particiones-vacia.png "tabla-particiones-vacia")


La info que nos da es que la tabla GTP empieza en el sector 2 (el sector 0
tiene un MBR "de por si acaso", el sector 1 tiene la cabecera de la tabla
GPT), y acaba en el sector 33.

El primer sector que puedo usar para meter la siguiente partición es el 34, también
puedo verlo con:

```
sgdisk -f 0.disk
```

También me dice que la alineación que se usa es de 2048, por tanto las 
particiones que creemos deben empezar (y acabar) en un sector que sea múltiplo
de 2048. Podemos verlo con

```
sgdisk -D 0.disk
```

Si queremos ver, de acuerdo a la alineación, en qué sector debe empezar la 
siguiente partición, podemos hacer:

```
sgdisk -F 0.disk
```

Esto al final nos dice que estamos haciendo la alineación recomendada de 
1MiB, aunque basta que las particiones estén alineadas en múltiplos de 4KiB.


Aunque aún no hemos visto nada de arranques, vamos a crear 3 particionados diferentes
en el disco para que sea arrancable mediante BIOS exclusivamente, mediante
UEFI exclusivamente y compatible con ambas.


## Particionado GPT para arranque BIOS
Necesitamos añadir una primera partición para que pueda instalarse GRUB,
que es el gestor de arranque cuya función es cargar el OS en RAM.

Esta partición va a ir entre el espacio que deja la tabla GPT y la primera
partición útil. Ojo, como queremos cumplir con la alineación, en seguida
nos damos cuenta de que si queremos la partición en ese espacio, no va a
seguir una alineación de 1MiB. Como basta que las particiones estén alineadas
en múltiplos de 4KiB, vamos a usar esa alineación para esta partición.

Para definir una nueva partición, el comando es:
```
sgdisk -n "numero-de-particion:sector-de-inicio:ultimo-sector 0.disk"
```

Si el valor "numero-de-particion" es 0, se coge el siguiente número de partición
disponible.

Por ejemplo,

```
sgdisk -n "7:5:4000" 0.disk
```

crería una partición (la 7), que empieza en el sector 5 y su último sector
es el 4000 (es decir puedo crear la siguiente partición a partir del 
sector 4001) [ojo que es solo un ejemplo, no tiene mucho sentido hacer una
partición así].

También puedo indicar los tamaños de la partición con unidades (`K, M, G`...):

```
sgdisk -n "0:2048:30G" 0.disk
```

Si no pones unidades se entiene que la unidad usada es "sector".
Además, puedo usar el signo `+` para representar que el número es relativo
al anterior: `0:+0:+1000` quiere decir que la partición que estoy creando
comienza 0 sectores más "a la derecha" que la anterior (es decir inmediatamente
después) y que tiene de tamaño 1000 sectores (comenzando a contar en el sector
en el que comienza la partición)


Para cambiar la alineación al definir particiones, podemos usar la opción
`-a`:

```
sgdisk -a 10 -n "0:100:1999" 0.disk
```

esto dice que la partición (que va a coger como número de partición el primer
valor disponible) y que empieza en el sector 100 y acaba en el sector 1999
lleva una alineación de 10 sectores (por tanto está alineada, aunque esta 
alineación no tiene sentido).

Para darle un FS a la partición, se usa la opción `-t`:

```
sgdisk -a 10 -n "0:100:1999" -t "0xf814" 0.disk
```

Es igual que el ejemplo anterior pero le da un FS con código `f814`.
Podemos ver los códigos de los FS con `sfdisk -L`. Si no decimos el código,
el FS por defecto será Linux (también conocido como ext4).

Finalmente, podemos dar una etiqueta a la partición (es conveniente para que
sea leible por humanos) con la opción `-c`:


```
sgdisk -a 10 -n "0:100:1999" -t "0xf814" -c "0:ETIQUETA" 0.disk
```

El ejemplo de arriba le da a la partción 0=la primera que encuentre la
etiqueta "ETIQUETA".


Con todo esto, ya podemos crear la primera partición para que se instale el 
GRUB, tiene que cumplir:
- estar alineada en múltiplos de 4KiB
- Estar entre la tabla GPT y el sector donde va a ir la primera partición útil 
    que seguirá una alineación de 1MiB
- tipo de partición "BIOS Boot Partition"
- etiquetarla como "BOOTBIOS"

Tras esto, creamos otras 3 particiones, que ya si llevarán la alineación
de 1MiB (habrá que cambiar la alineación). Las particiones irán seguidas
(una tras otra), tienen tamaños de 32M, 1G y 2G, la segunda partición
tiene de tipo de partición "Microsoft basic data", las otras 2 "Linux",
y están etiquetadas como "UNO", "DOS" y "TRES".

Una vez creadas estas particiones, modificamos la partición etiquetada como
"TRES" para que tenga tipo de partición "Linux Swap",
y creamos una nueva partición etiquetada como "CINCO" que ocupe el resto del
espacio disponible en el disco (ver cómo hacer eso invocando `sgdisk --help`)

Tras crear la particón "CINCO", borrarla (de nuevo, buscar el comando con
help).


Cambiar, en la tabla GPT, las entradas de las particiones "DOS" y "TRES"
(buscar la opción correct con --help)

(si en algún momento la hemos cagado y queremos empoezar de cero, el comando
`sgdisk -Z 0.disk`) es el que nos hace la limpia

## Particionado GPT para arranque UEFI
Crear un nuevo disco `1.disk`, y trabajar en él.

En este caso se crea una primera partición alineada "normal" (es decir, 1MiB)
que tenga 100M de tamaño y tipo de partición "EFI System Partition"; la 
etiquetamos como "EFI". Hacer las otras 3 partciones como antes.

La partición EFI alberga los arranques de los OSs que se pretendan instalar.
100M debería ser más que sufieciente en cualquier caso práctico.

## Particionado de arranque híbirdo
Crear un nuevo disco ``2.disk`` y trabajar en él

Consiste en combinar los 2 ejercicios anteriores:
- una partición alineada a 4KiB antes del sector 2048 etquetada como "BIOSBOOT"
    y de tipo de partición "BIOS Boot Partition"
- una pertición alineada a 1MiB de tipo de partición "EFI System Partition"
    etiquetada como "EFI" de 100M
-  las otras 3 particiones igual que antes