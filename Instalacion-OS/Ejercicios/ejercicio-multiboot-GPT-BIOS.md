# Multiboot BIOS y GPT

## Puto windows
no podemos hacr esto con OS windows. Windows te obliga a que si el arranque
es BIOS, el particionado es DOS, y si el arranque es UEFI, el partcionado es GPT.
Además, W11 solo funciona con particionado GPT (y por tanto arranque UEFI)

## Cómo proceder
Realmente no tiene mucha complicación, consiste en crear una partición
de tipo bios boot de 32KiB 
para que se instale ahí una de las partes del GRUB, y podemos hacer otra 
segunda partición de 32MiB para /boot/grub (si queremos independizarlo),
y luego ya las particiones para cada OS
(nota: VMs Ubuntu22.04 dan problemas al hacer este particionado; pero
consigo instalar varios Ubuntu20.04 sin problemas)

Este particionado es de arranque exclusivo BIOS

Estamos en este caso obligados a machacar el MBR con la primera parte de GRUB,
ya que el arranque es BIOS y debe ser capaz de encontrarlo; en el particionado
GPT las particiones activas no tienen sentido

# Multiboot UEFI y GPT
(sin comprobar)

Ha que añadir una partición tipo EFI y FS FAT32 de 100MiB.
Si quieres, la partición para GRUB igual que antes, 32MiB
Y el resto de particiones como sea necesario
(comprobar) el bootmanager (GRUB) debería ir a la EFI

# Multiboot híbirdo (y GPT)
para tener un disco que es arrancable via BIOS y via UEFI,
solo hay que combinar los 2 métodos anteriores, y crear una partición
bios boot y una partición ESP (y una para grub si quieres), y el resto,
como sea