# Restauración del sistema operativo
Vamos, al menos en un sistema Linux, no a hacer una restauración 
(por la dificultad técnica que conlleva, que supera los objetivos del curso)
del sistema operativo, si no que nos vamos a centrar en un apartado
complementario a la restauración, que son las copias de seguridad.

En Windows sí haremos restauración del sistema; y los conceptos de copias
de seguridad se aplican de manera idéntica aunque se ejecuten de maneras
algo diferentes.

Conceptualmente una restauración del OS consiste en revertir el estado del OS
a uno anterior. Esto viene motivado por lo siguiente: el OS consta, a grosso
modo, de programas y ficheros asociados a esos programas (configuraciones,
datos...)

En normal que en actualizaciones esos programas/ficheros cambien, en 
principio siempre para bien (mejoras en rendimiento, funcionalidad, seguridad...)
pero puede ocurrir que lo hagan para mal (una vulnerabilidad que se descubre,
un cambio que rompe funcionalidad...)

Incluso aunque no hubiese cambios que sean desastrosos, por motivos de
compatibilidad de aplicaciones a veces es deseable mantener ciertas versiones
de ciertas componentes que, por descuido o por negligencia, se cambian.

Si eso ocurre, es deseable tener una manera de volver "hacia atrás", ya
que desde el momento en que se instala el OS vamos solo "hacia delante"
(es decir introduciendo cambios que se sienten "irreversibles", no porque
no se puedan revertir, si no porque suele ser complicado tener claro qué
es todo lo que se ha cambiado y revertirlo)

Un punto de restauración es una manera "fácil" de hacer ese cambio hacia atrás;
esencialmente consiste en sacar una "foto" al estado del OS en un momento dado
y volver a esa "foto" en caso de que queramos/necesitemos, en lugar de 
tener que volver a instalar el OS desde cero y avanzar hacia delante hasta volver
al estado desdeado del sistema

# Copias de seguridad
Complementario a los puntos de restauración del sistema están las copias
de seguridad; éstas no hablan tanto del OS (programas) si no de los archivos
(los datos) que se almacenan en disco (y a los que se accede via el OS)

Esa información puede perderse por multitud de motivos (virus, descuido, se rompe el disco duro...)
y si se trata de información crítica (ya no pensar en una empresa y los datos
empresariales que pueda contener, imagina que tienes DNI, certificado digital,
título de estudios... lo que sea almacenado en tu disco y se pierde)
y lo que hay que hacer es tomar medidas preventivas asumiendo que esa pérdida va
a ocurrir y, en caso de que ocurra, poder recuperar toda o casi toda la
información

A nivel concpetual los backups deberían hacerse de manera periódica
(esta periodicidad la dictarán muchos factores, como la criticidad de la 
información, la necesidad de tener disponiblidad de esa información con
rapidez...) e idealmente automatizada (via un programa)

Otra cuestión a tener en cuenta sobre las copias de seguridad es dónde se va
a almacenar esa información (y como, pero es más sencillo: comprimido), ya
que al final no tiene mucho sentido almacenar en el mismo disco donde está
la información (si se rompe, el backup también se rompe), por tanto ya se plantea
de base el tener otro dispositivo de almacenamiento (más dinero gastado)
para almacenar información. Por supuesto esto es una cuestión que solo
tiene respuesta para cada caso específico, ya que entran en juego variables
demasiado diversas como presupuesto, tamaño necesario para almacenamiento...

Otra cuestión a tener en cuenta es qué se va a respaldar. Técnicamente,
lo que te de la gana; para ser prácticos (y teniendo en cuenta que estamos en
un sistema Linux), a tener en cuenta lo siguiente:

- los datos de usuario, es decir, todo lo que hay por debajo de /home
- ficheros de configuración, es decir, cosas que haya debajo de /etc
- ficheros de monitorización, es decir, lo que haya por debajeo de /var/log

Sobre los datos de usuario entiendo que no hay mucho que desarrollar; sobre 
los ficheros de configuración, es por el hecho de que si llegamos al punto
de tener que reinstalar el OS, los ficheros de configuración volverán a su
por defecto; volver ac onoseguir la configuración deseada puede ser un 
trabajo largo y estúpido, sobre todo si ya se tiene ese fichero y se puede
plantar; respecto a los logs, la razón lógica es que si algo ha ido mal,
estaría bien saber qué es, y eso solo lo voy a averiguar siguiendo los
logs

Otra cuestión a tener en cuenta es qué tipo de backup se va a hacer, esto
suele ser un tema difícil de entender.

## Tipos de backup: completo
Este no tiene miesterio, consiste en copiar todo lo que quiero copiar
Siempre habrá que tener al menos un backup de este tipo

Pros: recuperar todo es fácil (porque ya está todo en un sitio)
Cons: es el que más tiempo lleva de hacer y el que más espacio consume

## Tipos de backup: diferencial
Para hacer este backup, necesito tener antes un backup completo
En este backup solo guardas los ficheros que se han modificado desde
el último backup full. Por ejemplo, tengo backupeados mi DNI, mi título 
de FP, mi diario personal y mi CV, a fecha 23 de febrero

Voy a hacer un backup diferencial de mis ficheros personales, ¿qué debería
respaldar? Pues seguramente mi diario personal (que es algo que se espera
que cambie a diario) y seguramente también mi CV (que es algo que suele
actualizarse cada cierto tiempo). El DNi es algo que cambia cada mucho tiempo,
y el título simplemente no cambia, así que (seguramente) no haya que respaldar
esos ficheros

Pros: ocupa menos que un full, es más rápido de hacer, es fácil hacer una
restauración porque solo necesito los ficheros del último full más los ficheros
del último diferencial
Cons: Tarda más que el incremental y ocupa más, hay que hacerlo con más frecuencia
que un full

## Tipos de backup: incremental
Para este backup necesito tener otro, ya sea full o diferencial.
La idea de este bakcup es ue respaldo todo lo que ha cambiado desde el último
backup. Con el ejemplo de antes, si tengo mi CV, título, DNI y diario,
lo normal es que en un backup incremental solo guarde una copia del diario,
que es eguramente lo único que ha cambiado desde el último backup
Pero si al día siguiente resulta que renuevo el DNI, y no me apetece escribir
en mi diario, el siguiente backup incremental solo guarda el DNI (porque 
lo único que ha cambiado desde el último backup es ese fichero)

Pros: es el que menos ocupa, el más rápido
Cons: hay que hacerlo con mucha más frecuencia que un diferencial,
y la restauración es complicada, ya que necesitas el último full más
todos los incrementales que se hayan hecho para tener la última versión de todo

En general la estetegia de backup es algo que combina estas 3, por poner
un ejemplo simple, se puede considerar hacer un backup full mensualmente,
un backup diferencial semanalmente y un backup incremental diariamente

De esta manera, en caso de pérdida, la restauración de los ficheros es
"sencilla" porque solo necesito coger el full, el diferencial último y todos
los incrementales desde el diferencial último

## Cómo hacer backups de ficheros
Primero, 2 conceptos que existen en el mundo UNIX: comprimir y empaquetar

### Coprimir
Consiste en que la información de un determinado fichero pase a ocupar menos
sin perder infomación.

#### Compresores habituales
- gzip, con extensión asociada .gz
- bzip2, con extensiones asociadas .bz2 y .bzip2
- xz, con exensión asociada .xz. Es el que se usa hoy en día, el mejor en cuanto
    a tamaño conseguido al comprimir y a velocidad de descompresión
-zstd, extensión asociada .zst

La forma de usar todos estos comporsores es igual (salvo alguna opción que 
cambie de nombre), así que vamos a hacer un
ejemplo con xz y para el resto de compresores se puede calcar o, en caso de duda,
consultar la ayuda con la opción -h.

Para usar el programa, primero tenemos que crear un fichero, por ejemplo
`lorem.txt` donde metemos texto del Lorem Ipsum.

Una vez tenemos el fichero, simplemente basta llamar al programa:

```bash
xz lorem
```

Y nos generará un comprimido, eso sí, elminando el original. Opciones habituales
que se suelen usar con estos programas (de nuevo, puede que la llamada a las
opciones cambie de uno a otro, pero estarán):

- `-0 .. -9`: indica el grado de compresion que queremos aplicar, siendo 0
    el menor y 9 el mayor. A más grado de compresión, mas pequeño es el
    fichero resultante, pero más tiempo llevará y más recursos consumirá
    del equipo
- `-v`: verbose, hace que el programa me cuente que está haciendo
- `-k`: keep, no elimina el fichero original
- `-l`: da info sobre el tamaño del comprimido y el original, así como el grado
    de compresión
- `t`: test, checkea que el comprimido es correcto (es decir, que no he perdido
    información al comprimir)
- `-d`: descomprimir

Estos son los más comnues, pero también tenemos en linux `zip` y `unzip`
para comprimidos zip, rar y unrar y p7zip


### Empaquetar
Consiste en reunir varios ficheros en uno solo. El programa que lo hace es
`tar`. Creamos un par de ficheros fich1.txt y fich2.txt, la manera de
usar tar es:

```bash
tar -cf empaquetado.tar lista-de-ficheros-a-empaquetar
```

para este ejemplo:

```bash
taf -cf tarado.tar fich[12].txt
```
nos generará un fichero `tarado.tar`que contiene a los 2 ficheros
Ojo que esto no comprime, es decir, el tamaño de `tarado.tar` es la suma
de los tamaños de fich1 y fich2

Las opciones más comunes que se usan con tar (hay muchas):
- `-f`: indica el fichero contenedor, ya sea empaquetando o desempaquetado
- `-c`: indica que voy a empaquetar
- `-x`: indica que voy a desempaquetar (en el directorio de trabajo)
    también puedes desempaquetar uno o varios de los ficheros indicándolos
    `tar -xf tarado.tar fich2.txt`
- `-t` dice qué hay en el paquete
- `-v`: verbose, se suele usar siempre para que tar de más info


Si tenemos directorio tar preservar también el árbol

```bash
mkdir -p DIR{1/DIR11,2}
touch DIR1/{fich1,DIR11/fich11} DIR2/fich2
tar -cf dir-tarados.tar DIR[12]
tar -vtf dir-tarados.tar
```

Existe otra opción, `-C`, para trabajar más cómodamente con directorios:
Si digo a tar:

```bash
tar -C /tmp -vcf dirs.tar DIR[12]
```

El paquete se generará en mi pwd, pero asume que bajo `/tmp` tenemos la
estructura de directorios que vamos a empaquetar. Si queremos desempaquetar,
en nuestro pwd creará el directorio `/tmp` y luego meterá el resto ahí,
pero obviamente podemos, al desempaquetar, decir en qué directorio
quiero que desempaquete:

```bash
tar  -C $HOME/desempaq -vxf dirs.tar
```

Finalmente, es habitual empaquetar y comprimir el empaquetado; para ello tenemos
que añadir la opción `-a` junto con una extensión de compresión en el 
nombre del empaquetado

```bash
tar -avcf dirs.tar.xz DIR[12]
```

Para desempaqutar un comprimido, también hay que suar la opción `-a`, y
tar se encarga de detactar cuál es la extensión del comprimido

Finalmente, una opciñon muy útil es `-T`, que permite indicar un
fichero de texto plano que contiene el listado de cosas a empaquetar, en
vez de tener que pasarla por argumento:

```bash
tar -vacf empaq.tar.xz -T lista-a-empaquetar.txt
```