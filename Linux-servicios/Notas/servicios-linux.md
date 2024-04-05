# Servicios
Los servicios o deamons son procesos que corren en segundo plano
(es decir, sin intervención del usuario) y el sistema depende de muchos
ellos para funcionar bien. Normalmente están haciendo algo por tu OS
o están a la espera de recibir una petición.

La mayoría de los servicios se inician el bootear el sistema. En el booteo
del sistema, existe un priper proceso que se lanza (el parent) que 
tradicionalmente era `init`, pero en las distros más comunes (Arch, RedHat,
Debian y derivadas , ie, Ubuntu, Fedora, Manjaro...) se cambió por el proceso
`systemd`

Para saber si estamos en un sistema init o systemd, basta con lanzar el
comando

```bash
pstree | head -5
```
si aparece systemd o init al pprio nos confirma cual ha sido el primer proceso

## Systemd
Brevemente, systemd se define como un gestor del sistema y de 
servicios. En este caso lo vamos a usar como gestor de servicios

## Units
Las units son un estandar que usa systemd para la gestión del sistema.
Puede haber varos tipos de unidades pero las que nos interesan en este momento
son las que terminen en `.service`, es decir, los servicios.

Para ver las unidades, lanzamos `systemctl`, que nos mostrará todas,
pero si queremos filtrar por un tipo concreto de unidad podemos lanzar:

```bash
systemctl --type=service
```

para ver las unidades de servicios, en este caso concreto

## Un ejemplo: ssh
Todos los servicios funcionan más o menos igual, vamos a seguir un ejemplo
de un servicio concreto para ver cómo gestionarlos.

En este caso el servicio que vamos a gestionar es OpenSSH, que es el conjunto
de herramientas que se usan para obtener conexiones remotas seguras entre equipos.

Es una herramienta con arquitectura cliente-servidor, es decir, como lo que 
vamos a hacer es conectarnos remotamente a otro equipo, tenemos qu tener el
cliente habilitado en el equipo en el que estemos trabajando y el servidor
habilitado y configurado en el equipo al que nos vamos a conectar remotamente.

Para saber si tenemos intalado cliente o servidor, basta con hacer:

```bash
which ssh
which sshd
```

Lo normal es que el cliente esté instalado y e servidor no.
Para instalarlos, basta con

```bash
sudo apt install openssh-client
sudo apt install openssh-server
```

Una vez instalado el server (en el ordenador al que nos queremos conectar en
remoto) podemos ver el estado del servicio con

```bash
sudo systemctl status sshd
```

Lo normal sería que el servicio se autoactivara, pero tenemos las siguientes 
herramientas para activarlo, desactivarlo o resetearlo:

```bash
sudo systemctl start sshd
sudo systemctl stop sshd
sudo systemctl restart sshd
```

Los servicios puede que se inicien en boot o no; a veces nos interesa
tenerlo activo desde boot, a veces nos interesa activarlo/descativarlo
manualmente. Para ello está el concepto de `enable/disable`.

Marcar un servicio como `enable` es decirle al sistema que queremos que
lo lance en boot; disable es lo contrario

Para saber los servicios que están enable, hacemos

```bash
systemctl list-unit-files --state=enabled --type=service
```

Aunque para saber sobre un servicio concreto lo mejor es preguntar directamente 
al servicio con status

O podemos ignorar todos los argumentos para tener el listado de todas las units
instaladas en el sistema

Si por ejemplo hacemos 
```bash
sudo systemctl disable sshd
```

y reiniciamos el sistema veremos que el servicio ya no está activado por defecto
y debemos hacer un start manual para tenerlo activado.

**NOTA**: `sshd` es un alias, que podemos utilizar mientras el servicio
está enable; si lo disableamos, debemos referirnos al servicio por su nombre
de unidad: `ssh.server`

**PROBADO SOLO PARA MAQUINAS EN LA MISMA LAN**
Finalmente, con todo configurado, podemos comprobar que las conexiones ssh
son posibles. OJO que lo que vamos a hacer podría crear un fallo de seguridad.

Desde una máquina con el cliente ssh activado, a una máquina con el server ssh
activado, hacemos

```bash
ssh user@local-IP
```

y tras meter la password, veremos que tenemos conexión remota
La manera más correcta y más segura de hacer la conexión ssh no es con 
user+pass si no con la generación de un par de claves ssh public-private

**SIN TESTEAR** para generar las claves, en el ordenador cliente 
hacemos

```bash
ssh-keygen -t rsa -b 4096
```

Esto generará claves pública y privada. (id_rsa.pub es la pública).

Copiamos la pública a la máquina server con

```bash
ssh-copy-id user@local-IP
```

aunque lo más seguro sería copiar, via USB u otro medio, el contenido de
id_rsa.pub en el fichero (de la maquina remota) $HOME/.ssh/authorized_keys

Tras ello, deberíamos ser capaces de establecer conexión ssh sin tener que
escribir la password


Otra herramienta muy últil para la gestión de los servicios es `journalctl`
que es un servicio gestionado por systemctl para los logs (lo que va
escribiendo e programa que hace)

Si queremos ver los logs de sshd (tenemos que llamarlo ssh por un lio con los
alias) basta con hacer

```bash
journalctl -u ssh
```
y tendremos todos los logs de ese servicio concreto. Podemos ver TODOS los
logs de todos los servicios ignorando el filtro por -u, los 10 últimos 
con la opción `-n10`, y un stream de los logs con

```bash
sudo journalctl -f -u sshd
```

Se pueden hacer más filtros como por ejemplo de fecha/hora:

```bash
sudo journalctl --since "2015-11-10 14:00:00"
```