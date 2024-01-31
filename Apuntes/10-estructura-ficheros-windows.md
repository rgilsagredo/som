# organizacción de Windows
Windows no necesita demasiada instroducción en cuanto a cómo manejarse
en el OS pues tiene una GUI y en general la gente está acostumbrada a 
usar este OS sin problema (es decir, no necesito usar una CLI para poder
gestionar las cosas, todo tiene una GUI para poder tocarse)

Sí merece la pena saber, al igual que se hizo con Ubuntu, cómo se organiza
Windows y donde se espera encontrar las cosas.

Igual que los sitemas Linux, utiliza para la estructuración de la información
un sistema de ficheros y carpetas con forma de árbol, pero en este caso la
raíz es cada disco/volumen (C:, D:,..).

Windows también te da por defecto unas carpetas "especiales" que están
pensadas para almacenar ciertos tipos de ficheros (pero no estás obligado
a hacerlo, ni siquiera a usarlas), que serían las carpetas de documentos, 
musica, fotos, escritorio, descargas...

## program files
y también program files x86 en en donde se van a instalar (salvo que pasen
cosas raras) los programas. Cada programa suele tener su propia carpeta
con los ejecutables, bibliotecas y otros recusros que necesite.

Las aplicaciones diseñadas para sistema de 32 bits irán a la carpeta x86,
así se mantienen separadas aplicaciones diseñadas para cada arquitectura.

## Windows
es crítica para el sistema; entre otras cosas bajo /System32 tenemos
ejecutables del istema operativo (entre ellos el kernel), y librerías para
que las aplicciones funcionen, ficheros de configuración.

También dentro de esta cartepa está /Temp, la carpeta desiganda para
crear ficheros temporales que crean las aplicaciones y usuarios.

En la carpeta /System32/config se almacenan configuraciones y opciones para
el OS y las aplicaciones instaladas en el OS.

## Users
contienen, para cada usuario registrado en el sistema, una carpeta para ese
usuario. "Es" el lugar principal donde cada usuario va a almacenar
sus cosas. 

También contiene una carpeta oculta llamada AppData; que se dedica a almacenar
información y configuración sobre los programas que ha instalado o va a
usar ese usuario.

## Program Data
oculta por defecto, es similar a AppData de cada usuario pero para todos los 
usuarios; es decir, incluye datos compartidos y archivos de configuración
que son comune entre todos los usuarios del sistema