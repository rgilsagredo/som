# Ejercicios de usuarios y permisos en Windows:

## Primero
Crear estos usuarios/grupos:
![usuarios/grupos](./images/ej1.jpg "usuarios/grupos")

Y debe cumplirse:
- todos los usuarios deben evr un fichero en su escritorio que se llame
    "normas.txt"; los alumnos no lo pueden modificar ni elminiar pero sí consultar
- Una carpeta común a todos que se llame "Carpeta de todos"
- Una carpeta personal para cada usuario que se llame "Mis movidas"

## Segundo
Configurar la carpeta compartida para que:
- Los alumnos no la pueden elminiar
- Profesores ya lumnos pueden crear ficheros regulares pero no directorios
- Los profesores pueden borrar todos los archivos
- Los alumnos solo pueden borrar sus propios archivos

## tercero
Configurar las directivas de seguridad local para que:
- las passwords deben ser mínimo de longitud 8
- caducan a los 30 días
- Al tercer intento fallido de login, se bloquea al usuario 3 minutos
- Avisar de que la contraseña va a expoirar con 3 días de antelación
- EXTRA: eliminar la posibilidad de apagar el equipo si no se ha iniciado sesión