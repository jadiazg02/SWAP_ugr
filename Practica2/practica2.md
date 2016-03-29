#Practica 2: SWAP

##Compresion y copiado de contenidos entre dos maquinas por medio de shh.

En la primera maquina virtual introducimos el comando:

    tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/ssh1.png "En la maquina 1")

Tras lo cual vemos como en la máquina dos el contenido se ha copiado correctamente. Esta opcion es util para copiar contenido de una maquina a otra si por ejemplo en la primera no tenemos espacio para crear ese contenido comprimido. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/ssh2.png "En la maquina 2")


##Sincronizacion del directorio var/www por medio de shh y la herramienta rsync. 

Si no lo tenemos instalado instalamos en nuestas maquinas la herramiente rsync con el siguiente comando:
    
    sudo apt-get install rsync

Tras esto podemos comenzar con la sincronización, pero antes es conveniente ejecutar el siguiente comando para trabajar con todos los permisos sobre los ficheros. 

    sudo passwd root

Tenemos que tener en cuenta que deberemos introducir la contraseña dos veces aunque esta no se vea. El resultado podemos verlo en la siguente imagen.  

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/root1.png "Ejecucion del comando sudo passwd root en la máquina 2")

Para evitar posibles problemas podemos configurar el archivo /etc/ssh/sshd_config para permitir el login del root. Para ello procedemos así:

    vi /etc/ssh/sshd_config

Editamos la linea  o la añadimos si no aparece:

    PermitRootLogin yes 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/root2.png "Modificacion del parametro PermitRootLogin en la máquina 1")

Tras esto debemos reiniciar el servicio como de costumbre con el comando: service ssh restart

Ahora ejecutamos el siguiente comando en la máquina 2:

    rsync -avz -e ssh root@maquina1:/var/www/ /var/www/

Tras lo cual como podemos ver en la siguiente imagen hemos clonado el contenido de /var/www de la máquina 1 en la máquina 2.

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/rsync1.png "Ejecucion srycn en máquina 2")


Para comprobar que todo ha ido correcto, podemos acceder al servidor web de la maquina dos, donde al haberse copiado vemos como ofrece el mismo contenido que ofrecia la máquina 1, por lo que la clonacion ha ido bien. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/rsync2.png "Comprobacion rsync")


##Acceso sin contraseña para ssh. 

Para evitar introducir continuamente la contraseña cada vez que accedamos por ssh, podemos proceder así:

-Generamos una clave en la máquina 2:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/key1.png "Creacion de la clave en la máquina 2")


-Copiamos la clave a la máquina 1. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/key2.png "Copiado de la clave en la máquina 1")

Tras estos dos pasos podemos comprobar como podemos conectar por ssh sin introducir contraseñas lo que será muy útil para programar tareas de administracion automaticas. rsycn1

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/key3.png "Conexion sin pass por ssh")


##Automatización de tareas con crontab. 

Vamos añadir una tarea que sincronice el contenido del directorio /var/www entre las dos máquinas cada hora. Para ello editamos en la máquina 2 el archivo /etc/crontab y añadimos la siguiente tarea:

* */1 * * * sync -avz -e ssh root@192.168.0.1:/var/www/ /var/www/

El archivo debería quedar como en la siguiente imagen:


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP2/cron1.png "Configuracion de tareas automaticas con crontab")















    


