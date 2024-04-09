# ejercicios de monitorizacion

## 1
Personalizar el loggeo de los mensajes del kernel para que:

- los mensajes de nivel 0 a 3 se guarden **exclusivamente** en un fichero
    dedicado a esos mensajes (por ejemplo, los de nivel emerg podrían
    ir a un fichero que se llame `kern-emerg.log`)
- el resto de mensajes continuen escribíendose en el fichero kern.log
- La rotación de los mensajes de nivel 0 a 3 debe seguir la siguiente configuración:
    - se hará una rotación diaria para el nivel 3, semanal para el nivel 2,
    mensual para el nivel 1, anual para el nivel 0
    - los rotados siempre irán comprimidos
    - solo los admin podrán ver el nuevo fichero una vez se ha rotado
    - se hará rotación cuando el fichero alcance los 10MiB, pero no se hará
    hasta que no alcance los 5KiB
    - se harán 24 rotaciones para los mensajes de nivel 3, 7 para los de
    nivel 2, 30 para los de nivel 1, 12 para los de nivel 0

El resto de mensajes que no están en los niveles 0-3 seguirá la misma configuración
que se tenía anteriormente.

## 2
consultar con el sistema tradicional de loggeo los accesos fallidos al sistema

## 3
Con journalctl, averiguar cuales son los últimos 20 mensajes del kernel.
Además, dejar abierto un stream de 10 lineas que me vaya mostranso esos mensajes


## 4
Revisar los mensajes de cron de las últimas 4 semanas