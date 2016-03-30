#Ejercicio T3.1: 

##Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.

En sistemas Windows, podemos usar el comando route, con los siguientes argumentos:

    route [-f] [comando [addr] [MASK mask] [gateway] [METRIC cost]]

Este comando nos permite crear entradas en la tabla de encaminamiento o modificar y eliminar las existentes con lo que podremos enrutar nuestro tráfico como deseemos. 

En sistemas Unix, también tenemos el comando route aunque en este caso nos ofrece más parametros y configuraciones posibles. 

    route add [-net | -host] addr [gw gateway] [metric cost] [netmask mask] [dev device]

Para ejecutarlo necesitaremos privilegios de root. 

En cuanto a las herramientas gráficas en windows hay ciertos programas como por ejemplo WinBox con el que podremos realizar las mismas funciones que realizamos por linea de comandos pero en entorno gráfico. 


#Ejercicio T3.2:

##Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.

La herramienta esencial para esta tarea será tanto en windows como en linux el uso de un firewall. En linux, uno muy famoso es netfilter, que se controla a través de iptables donde podremos añadir reglas para evitar ciertos paquetes o incluso todo el tráfico. 