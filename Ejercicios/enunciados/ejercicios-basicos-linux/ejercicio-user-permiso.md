# Ejericicos sobre usuarios y permisos
1. Crear un usuario llamado moises y obligarle a cambiar la contraseña en el primer ingreso.
2. Evitar que cualquier otro usuario pueda espiar el directorio personal de moises. ¿Puede hacerlo el propio moises?
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


# Soluciones
## 1
Para crear el usuario, loggeados con un usuario `sudoer`

```bash
sudo adduser moises
```

Nos pedirá cierta info, la metemos. Comprobamos que podemos loggearnos
con el usuario. Tras ver que todo va bien, para obligar el cambio de password
hacemos

```bash
sudo passwd moises --expire
```

Y comporbamos que el intentar hacer login con el usuario moises nos obliga a
cambiar la password.

## 2
El directorio personal de moises es `/home/moises`. Ese directorio tiene, 
por defecto, ciertos permisos, que podemos averiguar con

```bash
ls -ld /home/moises
```

Vemos que son los siguientes:

```bash
drwxr-xr-x 14 moises moises 4096 mar 19 08:43 /home/moises/
```

En particular el usuario moises tiene control total sobre su directorio,
los usuarios del grupo propietario (moises) pueden ver contenido e incluso
acceder, y l resto de usuario tiene los mismos permisos. Para evitar que 
otros usuarios puedan ver lo que hay en ese directorio, solamente tenemos que
quitar los permisos de `r` de `others`.

Sin embargo, eso no impide que los usuarios se "metan" dentro del directorio;
para ello también deberíamos quitar el permiso `x` de `others` que es el 
que permite acceso a los directorios. Además, como `/home/moises` tiene
subdirectortios, lo más lógico es que también quitemos los permisos en
esos subdirectorios. Para hcer todas estas operaciones de una, basta con
hacer:

```bash
sudo chmod -R 750 /home/moises/
```

Y podemos comporbar que ya no es posible acceder ni saber qué hay en el
home de moises.

Para estar realmente seguros de que ningún otro usuario puede entrar ahí, 
bastaría comprobar que el único usuario del grupo moises el es propio moises,
para ello:

```bash
getent group moises
```

nos dirá que no hay más usuarios en ese grupo (al final de la linea se listan
los usuarios que no son moises que están en ese grupo, no debería haber ninguno),
por tanto moises es el único que puede acceder a su home.

## 3
Primero vamos a comrpobar que el grupo ya existe, para ello hacemos

```bash
getent group users
```

Y vemos que nos devuelve info. Para que el grupo principal de usuario `sara`,
que aún no hemos creado, sea user, podemos hacer lo siguiente:

```bash
sudo adduser sara --ingroup users
```

## 4
Basta con hacer

```bash
sudo adduser --system --home /srv/ftp ftp
```

## 5
Primero no loggeamos con `sara`, y creamos el directorio en su home
que se llame ``EJ5`` con

```bash
mkdir EJ5
```

Vemos que permisos tiene ese directorio con 

```bash
ls -dl ./EJ5
```

Veremos que ``otros`` tienen `r-x` de permisos. Eso es que pueden ver que hay 
en el directorio y meterse al directorio, pero no crear o borrar cosas.
Para que `others` puedan modificar los contenidos del directorio, deberíamos
otrogar el permiso `w`; pero entonces los usuarios pueden borrar cosas
de otros usuarios. La solución es usar los permisos espaciales, en cocreto
el `stickybit`, que hace que los contenidos solo los puede eliminar el 
propietario del directorio o del fichero.

Para dar los permisos que necesitamos:

```bash
chmod 01757 ./EJ5
```

## 6
Nos loggeamos con moises y hacemos

```bash
mkdir /tmp/PRUEBA
```

Vemos que el grupo propietario es `moises`. Si queremos cambiar el grupo 
propietario, haríamos

```bash
chown :www-data /tmp/PRUEBA
```

pero esa operació requiere permisos de superuser, que moises no tiene.
Entonces podríamos hacer 2 cosas, o que el superusuario cambie esos dueños,
o agregar al moises al grupo `www-data` mediante:

```bash
sudo usermod -aG www-data moises 
```

Y podemos hacer, con el usuario moises el comando de antes (no es necesario
sudo)

## 7
Para impedir que un usuario interactúe con el OS basta "quitarle" la shell,
para ello, con permisos de sudo, podemos hacer alguna de estas 2 opciones:

```bash
sudo usermod --shell /usr/sbin/nologin sara
sudo usermod --shell /usr/sbin/false sara
```

y veremos que ya no puede loggearse

## 8
Con un usuario creamos, en `~/Desktop`, ese fichero con con

```bash
touch script.sh
```
Comprobamos sus permisos, y lo modicifamos con nuestro editor de textos favorito
añadiendo las lineas. Para hacer que sea ejecutable, le damos el permiso 
(al usuario propietario), con

```bash
chmod 764 script.sh
```

Y lo ejecutamos con `./script.sh`

## 9
Los permisos `r-xrwxr-x` son `575`.

Como los permisos de los ficheros de usuario se crean mediante la resta
`0666-umask`, y vemos que el valor de umask es `0002`, y los permisos
que queremos son `rw------- = 0600`, basta hacer la resta para
sacar que debemos cambiar `umask a 0066`