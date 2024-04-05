# Ejercicio de servicios en linux
Instalar el servidor de base de datos MySQL v5.x en tu sistema y 
configurar que el servicio se lance manualmente. 

Adcionalmente, crear un grupo de usuario que van a ser lo "lanzadores"
del servicio y configurar y comporbar que solamente esos usuarios son 
capaces de inicial y parar el servicio

Loggearse remotamente a la máquina (tendrás que usar 2 máquinas) que tiene
el servicio instalado, usando una pareja de claves pub-priv, y abrir
una terminal con un stream de logs sobre el servicio. 

Iniciar una sesión interactiva con el cliente de mysql y comprobar que 
los logs se nos van actualizando en vivo (es decir, crear aluna DB, alguna
tabla...)