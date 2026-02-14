# [Sistema de Archivos (Filesystem Hierarchy Standard - FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
## que es un OS??

El **Sistema Operativo (SO)** es el software fundamental de una computadora, actuando como el **gerente de la máquina**. Su función principal es servir de intermediario entre el usuario y el **hardware** (procesador, memoria, disco duro), administrando todos los recursos para que los programas puedan funcionar de manera eficiente. Proporciona una interfaz para que podamos interactuar con la máquina y, lo que es clave, utiliza un **Filesystem** o sistema de archivos para organizar, guardar y encontrar toda la información en el disco duro, asegurándose de que los datos no sean un caos. Sin el sistema operativo, el hardware sería inútil.

## Que es un FS??

El `Filesystem` es la estructura que le permite al sistema operativo (Linux, Windows, macOS) `guardar`, `encontrar` y `gestionar` los datos en un disco duro de forma ordenada y eficiente.

La diferencia más fundamental: en Windows tienes varios "árboles" (uno por cada letra de unidad), mientras que en Linux tienes un solo "gran árbol" donde todo está conectado.

El FS en Linux distingue entre Minúsculas y mayúsculas, mientras que windows NO lo hace.


EJ:
```bash
df -h # Disk Fylesystem - Uso de disco por cada FS montado
du -sh /home/usuario/Documents  # Disk usage - Uso de disco de un directorio particular
lsblk # List block device - Muestra estructura física de los discos/particiones y la relacion de los FS con el hardware
ls -la  # List - Muesta detalles de los archivos com permisos/tamaño/....
```
