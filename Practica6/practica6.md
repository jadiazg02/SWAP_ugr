#Práctica 6: SWAP 

En esta práctica crearemos una configuracion RAID 1 por software sobre una de nuestras máquinas. 


##Creación de los discos necesarios:

Antes de nada, es necesario añadir dos discos a nuestra maquina virtual, para ello con la máquina apagada los añadimos normalmente a traves del software Virtualbox. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid1.png "Discos añadidos.")


Tras esto iniciamos la maquina  y comenzamos con la creación del RAID. 

##Configuración RAID:

El primer paso será instalar el software necesario. 

    sudo apt-get install mdadm

Tras esto debemos obtener informacion sobre si la máquina esta reconociendo nuestros discos para ello podemos usar el comando:

    sudo fdisk -l

Como podemos ver en la siguente captura de pantalla los discos son reconocidos en /dev/sdb y /dev/sdc aunque de momento no tienen un formato correcto. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid2.png "Discos añadidos.")



Tras esto debemos crear el RAID con el siguiente comando:

    sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc

Tras lo cual y aceptar un warning tendremos que el array en /dev/md0 esta iniciado. Con lo que solo tendremos que darle formato. Para ello usaremos el siguiente comando:

    sudo mkfs /dev/md0

Tras lo cual tendremos algo como la siguiente captura:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid3.png "RAID formateado.")

Tras esto debemos crear el directorio donde montaremos los RAID para ello usamos los comandos:

    sudo mkdir /dat
    sudo mount /dev/md0 /dat


Con el comando sudo mount podemos ver el resultado de estos comandos. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid4.png "Punto de montaje del RAID.")


Por último comprobamos el estado del RAID de la siguiente manera:

    sudo mdadm --detail /dev/md0

Lo que nos ofrecerá información como la siguiente:


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid5.png "Estado del RAID.")


Tras esto deberemos configurar nuestro sistema para montar el RAID por si solo al inciar, para ello, haremos lo siguiente:

-El primer paso pasa por obtener el UUID de los dispositivos de almacenamiento del RAID, para ello usaremos el siguiente comando:

    ls -l /dev/disk/by-uuid/

Lo que nos ofrece la siguiente información:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid6.png "UUID de nuestro RAID.")

Necesitamos el UUID de nuestro RAID, para añadir la siguiente linea en nuestro archivo /etc/fstab/ y que este se inicie solo al inciar el sistema. 


    UUID=8d48e303-d381-4cfe-9082-65c3e1d748ee /dat ext2 defaults 0 0

##Pruebas del RAID:


Podemos provocar un fallo con el siguiente comando:

    sudo mdadm --manage --set-faulty /dev/md0 /dev/sdb

Tras lo cual podemos ver como el disco b nos ofrece un estado de fallo:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid7.png "Disco B con un fallo.")


También podemos "retirar" en caliente un disco, en este caso el B ya que esta dando fallos:

    sudo mdadm --manage --remove /dev/md0 /dev/sdb

Tras lo cual vemos como solo aparece un disco:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid8.png "Disco B retirado.")


Por ultimo añadiremos de nuevo el disco, "ya arreglado".


    sudo mdadm --manage --add /dev/md0 /dev/sdb


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid9.png "Disco B añadido y reconstruyendose.")

Tras un momento, el disco B en lugar de estar en estado "spare rebuilding" estará de nuevo operativo:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/raid10.png "Disco B de nuevo operativo.")

##Servidor NFS:

Lo primero que debemos hacer es instalar los paquetes necesarios tanto en los "clientes" como en los servidores:

    apt-get install nfs-common nfs-kernel-server

Tras esto en la máquina en la que tenemos montado en RAID añadimos lo siguiente:


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/nfs1.png "Configuración nfs.")


Con esta linea ponemos a disposicion de la máquina .3 nuestro RAID. 

Tras esto podemos iniciar nuestro servicio NFTs en la máquina servidora.

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/nfs2.png "Inicio de nfs.")

Tras lo cual podemos comprobar en la máquina cliente como tenemos acceso a esta carpeta:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP6/nfs3.png "Pueba de nfs.")

