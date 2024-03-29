# ejercicios de permisos
1. Mejorar la privacidad de nuestro usuario para que solamente él pueda ver los archivos contenidos en su espacio personal.
2. Volver a dejar el directorio personal como estaba y crear dentro un subdirectorio al que no puedan acceder los usuarios que no pertenezca a mi grupo principal.
3. Crear un directorio propiedad de root al que solo puedan acceder los usuarios del grupo «netdev» (además del propio root).
4. Crear un directorio D, dentro un fichero f y permitir que todos puedan ver el contenido del fichero f, pero impedir que alguien (incluido uno mismo) lo pueda borrar.
5. Cambiar el grupo del directorio D a plugdev. ¿Es posible? ¿Y a disk? ¿Por qué?
6. Hacer que de forma predeterminada cada vez que creo un archivo sólo yo pueda leerlo.
7. Hago `chmod u+s /bin/rm` ¿Qué he hecho y que implicaciones prácticas tiene?
8. Supongamos que creamos un directorio /srv/ftp al que queremos que todos los usuarios por FTP puedan subir ficheros, pero no queremos que unos usuarios borren o modifiquen lo que pertenecen a los demás, ¿qué podemos hacer?
9. Tomando el caso anterior, supongamos que el directorio es para que los estudiantes suban sus trabajos y los profesores puedan corregirlos. En esta circunstancia, además, de que unos estudiantes no puedan alterar o borrar los trabajos de los demás, queremos que no se puedan copiar, ¿cómo lo logramos?
10. Supongamos que tengo un directorio llamado Fotos cuyo contenido (incluido subdirectorios) solo quiero que sea accesible por mi grupo de amigos. Para ello, el administrador ha creado el grupo amigos_usuario y me ha metido a mí y a mis amigos dentro. ¿Cómo lo hago?
    - (extra) Resuélvase de forma que se pretenda dar envidia, es decir, los usuarios que no son mis amigos. podrán ver que tengo fotos y cuáles son pero no abrirlas

## 1
Consiste en cambiar los permisos de directorio `/home/raul` (o el nombre
de usuario que tengas) y de sus subdirectorios para que `others` no
tenga ningún permiso

```bash
chmod -R 750 /home/raul
```

Adicionalmente, deberíamos comprobar que en el grupo propietario no
hay más usuarios que `raul`, para lo cual hacemos

```bash
getent group raul
```

Ojo que técnicamente deberíamos comprobar que no hay ningún otro usuaro cuyo
grupo propietario sea `raul`, pero hacer eso es algho más complejo.

## 2
Los permisos que tenían los directorios eran `0775`, así que basta hacer

```bash
chmod -R 755 /home/raul
```

Para crear el subdirectorio y que no puedan acceder usuarios que no sea de 
mi grupo propietario, podemos usar la umask. Vemos que umask tiene valor de
`0002`, por tanto por defecto los directorios que creemos tendrán 
permisos `0777 - umask = 0775`. Los permisos que queremos son `0776`
(u otra cosa, lo que quiero es que `others` no tenga el permiso `x`),
por tanto podemos hacer:

```bash
umask 0001
mkdir DIR
```

## 3 
Primero, creamos el dir, por ejemplo aquí:

```bash
mkdir /tmp/DIR
```

Luego, cambiamos usuario y grupo propietario con

```bash
sudo chown root:netdev /tmp/DIR
```

Y por último cambiamos los permisos con:

```bash
sudo chmod 770 /tmp/DIR
```

## 4
Creamos directorio y fichero:

```bash
mkdir /tmp/D
touch /tmp/D/f
```

Para que todos puedan ver el contenido del fichero, hay que dar, en el fichero,
el permiso de `r` en `others`

```bash
chmod 774 /tmp/D/f
```

Para impedir el borrado por cualquier usuario, lo que debemos hacer es quitar,
para todos los usuarios, los permisos de modifciación del directorio D:

```bash
chmod 555 /tmp/D
```

## 5
Sí podemos cambiar el grupo del directorio `/tmp/D` para que sea `plugdev` con

```bash
chown :plugdev /tmp/D
```

Pero no a `disk`. Es porque mi usuario pertence al grupo `plugdev` pero no 
al grupo `disk` (recuerda que los permisos son a discreción, solo puedo
dar cosas que tengo)

## 6
Para eso debemos cambiar `umask` para que los permisos por defecto
de un fichero que creemos sean `600`. Paa ello, la resta que se hace  para 
ficheros es `0666 - umask`, así que deberíamos cambiar umask a `0066`.

Pero este cambio solo perdura hasta que salgamos de bash. Si queremos
que esté para siempre (en nuestro usuario), debemos modificar
el fichero `~/.bashrc` y añadir la linea `umask 0066`

## 7 
Primero vemos qué permisos y que propietarios tiene el fichero `/bin/rm`.

Vemos que usuario y grupo propietario son root, pero cualquiera puede ejecutar
ese fichero (con sus permisos). 

Hacer `chmod u+s /bin/rm` hace que se añada
el permiso espacial de `setuid` al fichero, lo que hace es que cualquiera
que ejecute el fichero lo hará con los permisos del usuario propietario
(root), y no con sus permisos. 

A fefectos prácticos, esto hace que cualquier usuario pueda borrar cualquier
cosa, porque root puede borrar cualqueir cosa.

## 8 
El directorio ya existe; para que los usuarios puedan subir ficheros,
debemos dar el permiso de `w` para `others` en el directorio,
así e sposible que se añadan ficheros en ese directorio por cualquier
usuario:

```bash
chmod 757 /srv/ftp
```

Para evitar que los usuarios puedan elminiar ficheros que no son suyos,
hay que activar el permiso espacial `stickybit` en el directorio:

```bash
chmod 01757 /srv/ftp
```

## 9
Un directorio para que alumnos puedan subir ficheros.
No se puede borrar lo de otros
No se puede copiar lo de otros

Vamos a plantear esto un poco mejor. El admin del sistema provee a una clase
de un espacio donde los estudiantes pueden subir sus trabajos, y los profesores 
corregirlos. Obviamante, queremos que los estudiantes solo tengan control sobre
sus ficheros y no puedan eliminar ni copiar los de otros. El profesor tampoco
puede eliminar los ficheros, pero puede corregirlos (modificarlos).

Para hacer esto, primero creamos usuarios y grupos:

```bash
sudo adduser --no-create-home jefatura
sudo adduser --no-create-home profe
sudo adduser --no-create-home alumno1
sudo adduser --no-create-home alumno2
sudo addgroup profesores
sudo addgroup alumnos
sudo usermod --groups profesores --append profe
sudo usermod --groups alumnos --append alumno1
sudo usermod --groups alumnos --append alumno2
```

Estos comandos nos crean 4 usuarios, el admin que va a crear las cosas,
el profesor y 2 alumnos. También creamos un grupo para profesores y para
alumnos, y metemos a los usuarios en esos grupos.

Con el usuario jefatura creamos la carpeta compartida:
```bash
su jefatura
mkdir /tmp/clase
```
Veremos que los permisos de ese directorio están en 775 siendo usuario y grupo
propietario `jefatura`. Con esto, ni alumnos ni profesores podrán crear cosas
en el directorio. Para que puedan, tenemos que habiliat los permisos de 
`w` en el directorio con (el usuario jefatura)

```bash
chmod 777 /tmp/clase
```

Pero con esto los usuarios pueden tocar los ficheros de otros (borrar, copiar,
modificar). Para que solo los propietarios de los ficheros puedan tocar solo
lo suyo, activamos el stickybit en el directorio (con el usuario jefatura)

```bash
chmod 01777 /tmp/clase
```

Esto evita que los usuarios puedan borrar lo que no les pertenece (el 
propiatario del directorio sí podrá borrar cosas de cualquiera).

Pero se pueden seguir copiando ficheros. La clave para que no se puedan copiar
coas es que si se puede leer, se puede copiar. Por tanto, hay que quitar los
permisos de `r` para `others` en el directorio:

```bash
chmod 01773 /tmp/clase
```

Y eso hace que los `other` solo puedan ver sus cosas, en particular no
pueden ver lo de los demás. Pero esto plantea un problema, es que ahora el 
profe no puede ver lo de los alumnos. Pero la solución es fácil, agregamos al
profe al grupo de alumnos, y nos aseguramos que los usuarios del grupo
alumnos venga con el permiso de `w` por defecto en los ficheros

comprobar:
- alumnos pueden crear modificar y borrar cosas unicamente si son propietarios
- profesores pueden ver y modificar todo, no borrar nada

los alumnos pueden crear ficheros
los laumnos pueden modificar sus ficheros
los alumnos no pueden modificar los ficheros de otros
nadie puede borrar nada que no sea suyo
!! el profesor no puede modificar trabajos --> cambiarle a grupo jefatura
!! los alumnos pueden cat los ficheros de otros --> cambiar permisos de grupo alumnos
!! los alumnos pueden copiar trabajos de otros alumnos

Creo que la solucion va a ser: profe a grupo jefatura, quitar al grupo alumnos
permisos.

Creo que lo tengo así:
añadir a profesor al grupo de jefatura, para que pueda ver y editar cosas
modificar /etc/bash.bashrc añadiendo

```bash
if [ "$(id -nG)" = "alumnos" ]; then
    umask calcular_mask
fi
```

esto hace que para el grupo alumnos se ponga una máscara automática
Con ello, calcular la máscara para que los permisos de ficheros sea 700 o
algo así, la cosa es que no puedan tocar cosas del grupo pero si las suyas.
Y eso debería ser

## 10
PDTE