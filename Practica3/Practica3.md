#Práctica 3: SWAP 



Para comenzar a realziar la práctica debemos instalar una nueva máquina virtual que hará las veces de balanceador de carga. Para ello podemos seguir el tutorial que vimos en la práctica 1 de la asignatura. Importante no tener instalado ningún servicio que requiera el puerto 80 pues sino no funcionará correctamente. Una vez configurada la nueva máquina virtual y comprobado que tenemos conexión entre ellas, con el comando ping por ejemplo, podemos comenzar con esta práctica. 

##Balanceo de carga con nginx. 

Instalamos nginx en la máquina balanceadora de carga con los siguientes comandos:

    cd /tmp/
    wget http://nginx.org/keys/nginx_signing.key 
    apt-key add /tmp/nginx_signing.key
    rm -f /tmp/nginx_signing.key

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx1.png "Instalación nginx 1")


    echo "deb http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list

    echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list

Por último instalamos el paquete:

    apt-get update 
    apt-get install nginx

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx2.png "Instalación nginx 2")

Llegados a este punto debemos configurar el fichero de configuración de nginx para que funcione como un balanceador de carga. Para ello ejecutamos el comando:

    vi /etc/nginx/conf.d/default.conf

Y eliminamos todo lo que haya en su interior dejandolo como podemos ver en la siguiente imagen. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx3.png "Configuración final nginx")

Tras esto reiniciamos el servicio y si todo ha ido bien deberemos verlo como sigue:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx4.png "Configuración final nginx: reinicio")

Por último debemos comentar la linea de crontab añadida en la anterior página para permitir que el contenido de ambos index.html sea distinto y podamos ver como el balanceo de carga realmente funciona bien. Desde el anfitrión ejecutamos por tanto:

    curl 192.168.56.4

Y si todo funciona bien debemos ver que nos ofrece cada vez uno de los archivos iniciales de Apache en los cuales hemos cambiado el titulo. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx5.png "Comprobación nginx 1.")

Tras esto reiniciamos el servicio y si todo ha ido bien deberemos verlo como sigue:

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx6.png "Comprobación nginx 2.")


Ya tendriamos un balanceador round-robin perfectamente funcional, pero podemos mejorarlo aplicando pesos, por ejemplo, si una máquina es más potente que otra o simplemente esta menos (o más) saturada. Para ello modificamos el archivo de configuración nuestro upstream lo dejamos como sigue.

    upstream apaches 
    {
        server 192.168.0.1 weight=1;
        server 192.168.0.2 weight=2;
    }

Tras esta configuración el servidor la máquina dos atenderá el doble de peticiones que la 1. Nuestro archivo de configuración quedaría como vemos en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/nginx7.png "Configuración con pesos.")

##Balanceo de carga con haproxy. 

Para comenzar instalamos normalmente el software haproxy en la máquina que hace las veces de balanceador de carga. 

    sudo apt-get install haproxy

Tras esto tal y como hicimos con nginx, tenemos que modificar el archivo de configuración de haproxy para que se comporte como un balanceador de carga. Para ello escribimos el siguiente comando:

    vi /etc/haproxy/haproxy.cfg

Tras esto editamos el archivo de configuración que debería quedar como vemos en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/haproxy1.png "Configuración de haproxy.")


Una vez configurado paramos el servicio de nginxs para asegurarnos que el balanceador que esta funcionando es el de haproxy instalado en este punto, justo entonces podemos iniciar el servicio de haproxy. 

    sudo service nginx stop

    /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg

Si no ha habído errores podemos ver como funciona como en el anterior caso con el comando curl, vemos como igual que en el caso de nginx el titulo de la página va cambiando a medida que ejecutamos el comando. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/haproxy2.png "Comprobación haproxy1.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/haproxy3.png "Comprobación haproxy2.")



##Balanceo de carga usando pund. 

Lo primero que debemos hacer es parar el servicio de nginx como vimos anteriormente y el haproxy con el siguiente comando, para evitar así a la hora de hacer pruebas que los otros balanceadores correctamente configurados influyan en nuestro resultado. 

    ps aux | grep haproxy 
    kill -9 PIDdelProceso

Llegados a este punto instalamos Pound:

    sudo apt-get install pund 

Tras esto editamos su archivo de configuración:

    sudo vi /etc/pound/pound.cfg

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/pound1.png "Archivo configuración pound.")


Tras esto debemos editar el siguiente archivo ya que de otro modo pound no se pondrá en marcha. 
    
    sudo vi /etc/default/pound 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/pound2.png "Archivo configuración pound. 2.")

Tras esto iniciamos pound como demonio con el siguiente comando:

    sudo /etc/init.d/pound start

Podemos ver en la siguiente imagen como no ofrece errores. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/pound3.png "Puesta en marcha de pound.")

Para detenerlo debemos usar:

    sudo /etc/init.d/pound stop

Tras esto ya solo nos quedará comprobar que todo funciona correctamente para ello, desde la máquina anfitriona hacemos peticiones a el balanceador y esta nos debe ofrecer por turnos la web de la máquina 1 y la de la máquina 2. Como podemos comprobar en las siguientes imagenes. 


![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/pound4.png "Comprobación pound 1.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP3/pound5.png "Comprobación pound 2.")







