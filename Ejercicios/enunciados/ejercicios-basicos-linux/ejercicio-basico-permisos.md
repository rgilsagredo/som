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
PDTE
## 10
PDTE