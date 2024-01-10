# Ejercicio de particionado DOS en kernel Linux

## Preliminares
No vamos a usar un disco de verdad de la máquina, podemos usar que en Linux
todo son ficheros (en particular los discos) y crear un fichero que "sea"
un disco.

Para ello, vamos al directorio `/tmp/` y ejecutamos el comando

```
truncate -s 100G disco.disk
```

Eso crea un fichero disperso (realmente está vacío) de 100G.

Podemos ver que realemnte se ha creado si ejecutamos `ls` en el mismo
directorio `/tmp/`

Para ver info sobre nuestro disco, podemos usar la herramienta `fdisk`:

```
fdisk -l disco.disk
```

Para crear particiones en nuestro disco también usamos la herramienta `fdisk`,
que abre un programa interactivo:

```
fdisk disco.disk
```

Inmediatamente dirá que el disco no tiene tabla de particiones y que le 
crea una tabla DOS y le asigna un ID

Si tenemos cualquier duda, el propio programa nos dice que si pulsamos `m`
nos da info.

Para crear una nueva partición, vemos que el comando es `n`. Nos dice si
queremos que la partición sea `p` primaria o `e` extendida, y que escojamos
un número para la partición. 

Seguidamente, nos pide que indiquemos en qué sector queremos que comience
la partición, mostrando el primero que se encuentre disponible.

Tras ello, que demos un tamaño a la partición. Podemos dar el tamaño
diciendo el último sector de la partición, cuántos sectores (+) queremos que
ocupe, cuantos sectores (-) queremos que deje libres, o cuanto espacio queremos
que ocupe (+) o cuanto esopacio queremos que deje libre.

Con la secuencia de comandos 

```
n p 1 2048 +10G
```

se crea una Nueva partición Primaria que empieza en el sector 2048 y que
ocupa 10GiB (desde el sector 2048)

Podemos usar `p` de Print para que nos diga que particiones tenemos, deberíamos
ver la info.

Podemos cambiar el formato del FS con `t`, no hace falta saberse los códigos,
nos da un listado.

## Ejercicio
Dado un disco de 100G, paticionalo con tabla DOS de la siguiente manera:
- 3 particiones primarias consecutivas, de 512MiB, 10GiB y la última que deje
    50GiB de espacio sin usar
- 1 partición extendida de 20GiB
- La primera partición primaria tiene FS W95 FAT32
- La segunda primaria tiene FS ext4
- La tercera tiene FS HPFS/NTFS/exFAT
- Una partición lógica que ocupe 2/3 del volumen extendido, con FS Linux Swap
- Guardar la tabla de particiones en el disco, comprobar que todo está como 
    se pide con las utilidades
