# Gestión de recursos hardware
Ya se vió que los dispositivos de almacenamiento (y otros dispositivos que 
representan hardware) están representados por ficheros en `/dev`

Lo siguiente que nos interesa conocer es saber sacar información
sobe la RAM y la CPU

## free
Este comando, que se suele invocar con la opción `-h` para que muestre
la unidades en el mejor formato posible, muestra la cantidad de memoria
usada  disponible en el sistema, tanto RAM com SWAP

Recomiendo ver `man free` para una explicación detallada de qué es lo que
está mostrando cada columna de la tabla que se muetra, así como ver las
opciones que nos ofrece free

## /proc/meminfo
Es un fichero virtual que nos da un informe muy detallado de la memoria
que tenemos en el sistema.

Podemos verlo con `cat /proc/meminfo`, nos da mucha información así que
vamos a quedarnos con lo más relevante:

### cat /proc/meminfo | grep "Mem"
Infor general sobre la RAM: El total que tenemos, lo que no se está usando
(free) y lo que está disponible para ser consumida por procesos (available)

La principal diferencia entre Free y Available es que Available estima
la cantidad de memoria que podría estar diponible para procesos teniendo
en cuenta que podría recuperar memoria usada por buffers y caches; Free
dice lo que literalmente no se está usando


### cat /proc/meminfo | grep -e "Buffers" -e "Cached"
Buffers son una zona temporal de almacenamiento en la memoria, que se dedica
a almacenar datos miestras se transfieren de un lugar/proceso a otro
lugar/proceso. El uso má común de un buffer es operaciones I/O, en concreto
leer/escribir en un fichero. La idea es que en lugar de trasnmitir la info
byte a byte, vamos a almacenarla en un buffer y la mandamos todo de una. En
general no van a superar los 20MiB

Los caches son memorias muy rápidas en las que se almacena info (datos o
instruccciones) que van a ser accedidos de manera habitual. Aunque no
tiene por qué ser memoria física solamente, podemos hablar de cache 
para referirnos a una zona de memoria RAM done hay almacenada info
a la que se va a acceder habitualmente. El propósito del cache es
hacer que el sistema vaya más rápido ya que siempre es mejor leer 
de memoria que de disco (es mejor leer de memoria cahce que de RAM)

Con el comando del título podremos ver qué cantidad de memoria se destina a 
cada una de esas cosas


### cat /proc/meminfo | grep "Swap"
Swap es el backup de la RAM: si el OS se queda sin RAM, usará el espacio
reservado para SWAP, a costa de ser más lento, porque es espacio de disco.
Free y Total son evidentr que info dan, SwapCached es lo usado recientemente
Ver también /proc/swaps


### cat /proc/meminfo | grep -e "Active" -e "Inactive"
Active es la memoria que se ha usado recientemente, y no es buena para que 
nuevos procesos la reclamen. Inactive es la memoria que no se ha usado 
recientmente y está más disponible para ser usad por algún proceso

## /proc/cpuinfo
cualquier info que necesitemos sobre el procesador la vamos a encontrar aquí.

## sudo dmidecode
Muestra información sobre el hardware del sistema. Si queremos filtrar
sobre un hardware específico, usamos la opción `-t` junto con el palabro
para el hardware, el cual podemos averiguar con `dmidecor -t help`

## lsusb
lista el hardware conectado a buses USB. Usar la opción `-v` para una info
muy detallada

## lspci
Igual que lsusb, pero lista el hardware conectado al bus PCI