# ejercicio de usuarios en linux:
1. coprobar si existe "pepe" como usuario
2. crear a pepe como usuario y que su home esté en `/home/casa_de_pepe`
3. deshabilitar a pepe y que no pueda entrar en el sistema
4. crear a "bartolo" con UID=2002, y su grupo principal es el mismo que el de pepe
5. habilitar a pepe
6. cambiar el nombre de pepe a "donpepe". Cambiar su home a "/home/pepehome"
7. crear a "felicia" pero no darla un home
8. añadir a "donpepe" a los grupos "floppy" y "audio"
9. borrar a "felicia"
10. deshabilitar a "bartolo" cambiandole la shell
11. Rehabilitar a bartolo y forzar a que tega que cambiar su contraseña
12. bloquear a bartolo
13. Hacer que bartolo deba cambiar su password cada 3 meses, pero no lo pueda
    cambiar en la primera semana tras un cambio.
14. Hacer que el password de bartolo caduque el 30 de junio de 2024
    y que su cuenta se desactive si está inactivo durante más de 2 semanas

## 1
Para saber si existe un usuario, basta consultar el fichero de usuarios
con

```bash
getent passwd pepe
```

y ver si obtenemos algún tipo de información.

## 2
Para crear al usuario y que su home sea `/home/casa_de_pepe`, hacemos

```bash
sudo adduser --home /home/casa_de_pepe pepe
```

## 3 
Para que pepe no pueda entrar al sistema, basta con que le quitemos la shell,
para ello podemos hacer:

```bash
sudo usermod --shell /usr/sbin/nologin pepe
sudo usermod --shell /usr/sbin/false pepe
```

## 4
Para crear a bartolo con un UID específico, hacemos:

```bash
sudo adduser --uid 2002 --ingroup pepe bartolo
```

## 5
Para rehabilitar a pepe, basta con darle de nuevo una shell:

```bash
sudo usermod --shell /bin/bash pepe
```

## 6
Para cambiar el nombre de pepe a donpepe, hacemos

```bash
sudo usermod --login donpepe
```

Ya que el nombre de usuario es el nombre que se usa para hacer login con el 
usuario.

Para cambiar su home, hacemos

```bash
sudo usermod --home /home/pepehome --move-home donpepe
```

## 7 
para crear a felicia pero dejarla homeless, hacemos

```bash
sudo adduser --no-create-home felicia
```

## 8
para añadir a donpepe a esos grupos, hacemos

```bash
sudo adduser -aG floppy,audio donpepe
```

## 9
Para borrar a felicia, y de paso, todo lo que tenga, hacemos

```bash
sudo deluser --remove-home --remove-all-files felicia
```

## 10
Igual que con pepe antes

## 11
Para rehabilitarle, como antes con pepe.

Para que tenga que cambiar su contraseña en el proximo login,

```bash
sudo passwd --expire bartolo
```
## 12
Para bloquearle, hacemos:

```bash
sudo passwd --lock bartolo
```

## 13
Para ello, tenemos que jugar con mindays y maxdays

```bash
sudo passwd --mindays 7 --maxdays 90 bartolo
```

podemos comprobar los cambios con

```bash
sudo chage --list bartolo
```

## 14
Para ello, tenemos que hacer:

```bash
sudo chage --expiredate 2024-06-30 --inactive 14 bartolo
```