#Ejercicio T5.1:

##Buscar información sobre cómo calcular el número de conexiones por segundo.

Para ver el número de conexiones podemos usar el comando apache2ctl, concretamente de la siguiente manera:

    apache2ctl status |grep request

Hay otra herramienta donde podremos obtener más informacion sobre las conexiones llamada, ipstate, la cual consulta directamente la tabla de conexiones de Linux. Con ella podemos ver el número de conexiones a una IP y puerto en concreto usando por ejmplo:

    iptstate -1 -d 192.168.0.100 -D 80


#Ejercicio T5.3: 

##Buscar información sobre características, disponibilidad para diversos SO, etc de herramientas para monitorizar las prestaciones de un servidor.

Por ejemplo en windows, si nuestro servidor es por ejemplo el server 2008 podemos usar el powershell el comando:

    perfmon

Lo que nos lanzará el monitor del sistema que ofrece informacion relevante sobre el uso y estado de este. 

Para monitores de recursos hardware tenemos distintas opciones como por ejemplo: CPUThermometer y HWInfo en Windows y PSensor o Hardinfo en linux, por su parte Mac tiene herramientas potentes como por ejemplo TGPro. 

Otros monitores de uso más estendido pueden ser Munin o Ganglia. 


