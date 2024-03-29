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