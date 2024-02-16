# Ejericicos sobre usuarios y permisos
1. Crear un usuario llamado moises y obligarle a cambiar la contraseña en el primer ingreso.
2. Evitar que cualquier otro usuario pueda espiar el directorio personal de moises. ¿Puede hacer el propio moises?
3. Crear un usuario llamado «sara» cuyo grupo prinicpal sea «users». Comprobar, además, que este grupo ya existe.
4. Crear un usuario del sistema (o sea, un usuario que no representan a una persona física) llamado «ftp». Su directorio personal debe ser /srv/ftp.
5. Hacer que «sara» cree un subdirectorio dentro de su directorio personal en el que otros usuarios puedan dejar ficheros, pero que unos usuarios no puedan borrar los ficheros dejados por otro.
6. Crear con moises un directorio llamado «PRUEBA» dentro del directorio temporal. Intente hacer con moises que el grupo propietario de este directorio sea «www-data». ¿Es posible? Justifique la respuesta.
7. Impedir que «sara» pueda acceder al sistema.
8. Escriba en un fichero llamado «script.sh» el siguiente contenido:
```bash
#!/bin/sh

echo "Hola, mundo!!!!"
```
permite ejecución y ejecútalo.
9. ¿Cuál es el equivalente numérico a los permisos r-xrwxr-x? ¿Cuál es la máscara que debe definirse para que los ficheros de los usuarios se creen con permisos rw-------?
