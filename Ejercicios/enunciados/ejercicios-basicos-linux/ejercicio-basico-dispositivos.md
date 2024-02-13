# ejercicio

1. Enchufar un disco de 4G a la máquina virtual.
2. Crear tres particiones primarias en el disco duro de 1G, 2G, 1G
3. Formatear las particiones con las siguientes características: 
    1. ext4, label=UNO
    2. xfs, label=DOS
    3. ntfs, label=WINDOWS
4. montar la particion UNO como solo lectura
5. Crear una entrada en fstab para que se mone automáticamente WINDOWS en
    /home/windows