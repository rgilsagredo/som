# Ejericicos paths

No cambiar de directorio salvo que se indique

1. ir al directorio general de configuraciones
2. sin salir del directorio, consultar lo que tiene `/` usando rutas relativas
3. de manera relativa, ir al directorio que tiene los datos de los usuarios
4. consultar el contenido del subdirectorio de tu usuario, que está en su `~`, es 
    oculto y se llama config, de manera relativa y absoluta
5. averiguar donde estoy
6. moverse al directorio `local` que está en `usr`
7. suponiendo que se han hecho las cosas bien, ¿se ha instalado algún
    programa manualmente sin usar un gestor de paquetes?
8. volver, relativamente, al direcotrio personal
9. mostrar el contenido del directorio raíz de manera relativa
10. obtener un listado de "cuántos usuarios" tiene el sistema

## solución
```console
cd /etc
```

```console
ls ./..
```

```console
cd ./../home
```

```console
ls ./user-name/.config/
ls /home/user-name/.config/
```

```console
pwd
```

```console
cd /usr/local
```

```console
ls /opt/
```
dependerá la respuesta si hay ficheros aquí o no

```console
cd ./../../home/user-name
```

```console
ls ./../../
```

```console
ls /home (absoluto)
ls ./.. (relativo)
```