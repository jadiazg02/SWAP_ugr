#Práctica 5: SWAP 

En esta práctica crearemos una base de datos y posteriormente la replicaremos entre los servidores de nuestra granaja web con diferentes configuraciones. 


##Creación de la base de datos:

Para crear nuestra base de datos, accedemos a la primera máquina y nos logueamos en mysql. Para ello usamos los siguientes comandos. 

    mysql -uroot -p

Introducimos nuestra contraseña.

    mysql> create database contactos;
    mysql> use contactos;

Tras esto creamos dos tablas e insertamos datos de prueba de la manera que podemos ver en la siguiente imagen. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql1.png "Creacion de las tablas e insercción de los datos.")


Al final de la captura, podemos ver como las tablas han sido creadas. Ahora podemos consultar sobre estas:


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql2.png "Consulta sobre la base de datos.")


##Replicar una BD MySQL con mysqldump.

En este punto, mediante el uso de mysqldump cargaremos los datos de la base de datos actual sobre el siguiente servidor, (el esclavo). Para ello procedemos de la siguiente manera:

-Paso 1: Nos aseguramos de que no se esta cambiando ningun dato en este mismo momento para ello:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql3.png "Detenemos la actualización de datos.")

-Paso 2: Volcamos en el servidor maestro los datos en un archivo sql:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql4.png "Ejecución y resultado del comando mysqldump")

-Paso 3: Tras esto, debemos copiar este archivo en nuestro segundo servidor. Una vez en este, deberemos crear la base de datos con el mismo nombre que tendriamos en el server 1, ya que mysqldump obtiene las consultas de la insercción los datos y la creacion de las tablas pero no la creación de la bd.

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql5.png "Copiamos el contenido de la BD del server 1 al 2 en un script sql")

-Paso 4: Por último debemos crear la base de datos en el server 2 y ejecutar el script, para ello procedemos como podemos ver en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql6.png "Creamos la base de datos en el server 2.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql7.png "Copiamos los datos en el server 2.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/sql8.png "Comprobamos que todo ha ido correctamente, ejecutando una consulta.")


##Replicación de BD mediante una configuración maestro-esclavo.

Editamos con vi el archivo /etc/mysql/my.cnf y modificamos los siguientes campos:

Comentamos el campo bind-address 127.0.0.1

Descomentmos o editamos los siguientes campos de manera que queden de la siguiente manera:

    log_error = /var/log/mysql/error.log
    server-id = 1
    log_bin = /var/log/mysql/bin.log

Por último, guardamos el doc y reiniciamos el servicio. Si todo ha ido bien, deberiamos ver algo similar a la siguiente captura:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/maestro1.png "Comprobación de que los cambios se han realizado correctamente.")


En el siguente paso, vamos a la máquina 2 y configuramos el "esclavo" de la misma manera que el maestro pero con server-id=2. Tras esto, podemos reinicar el servicio para seguir configurando el esclavo. Pero primero accedemos al maestro y ejecutamos los comandos que podemos ver en la siguiente captura de pantalla, antes de nada deberemos haber creado un usuario "esclavo". 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/maestro2.png "Configuración del usuario 'esclavo' en el maestro.")

Tras esto en el esclavo configuramos los datos acorde a la informacion del maestro. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/maestro3.png "Configuración del esclavo.")

Por último si todo ha ido bien, ejecutando el SHOW SLAVE STATUS podemos ver que todo ha ido bien  y que tendremos nuestro esclavo funcionando. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP5/maestro4.png "Comprobación.")








