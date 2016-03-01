# Ejercicio del tema 1 SWAP

## Comparación entre servidores, Apache, nginx, Cherokee..

Si el contenido que vamos a usar en nuestra web es contenido estático sin lugar a dudas lo más recomendado es usar el servidor **nginx** el cual es un servidor web muy ligero y que sirve contenido estático de manera muy veloz y eficiente, si por el contrario necesitamos contenido dinámico este servidor ya no será nuestra mejor opción, y tendremos que decantarnos por otras opciones como por ejemplo el servidor por antonomasia **Apache**, el cual tiene una gran cantidad de funciones y configuraciones para que podamos ejecutar prácticamente todo lo que podamos imaginar, ademas es muy robusto y fácilmente modificable por lo que si necesitamos que nuestra web sea estable y tendremos contenido dinámico esta puede ser nuestra mejor opción. 

Si por el contrario lo que tendremos serán aplicaciones web deberemos decantarnos por **Cherokee** el cual es un servidor web que pretende asemejarse a Apache en cuanto robustez pero siendo más ligero añadiendo funcionalidades como opciones para desplegar una nueva aplicación web rápidamente o una interfaz web muy sencilla y elegante desde la que podremos manejar todo de manera fácil. 

Si por el contrario necesitamos servir solamente grandes cantidades de contenido estático podemos decantarnos por **thttpd** el cual se caracteriza por ser simple, pequeño, portátil, rápido, y seguro, ya que utiliza los requerimientos mínimos de un servidor como por ejemplo Apache. 

Por último, usando node.js conseguiremos ejecutar y compilar del lado del servidor código javascript a velocidades increibles, por lo que si nuestro contenido tiene un alto contenido, valga la redundancia, en código javascript será muy útil que nos decantemos por **node.js**. 


Para concluir, remarcar que muchas de estas tecnologías no son excluyentes y podremos tener ambas trabajando para hacer que nuestro contenido o aplicaciones se sirvan de una manera más rápida y veloz de manera que el servicio que demos a un usuario final sea de mayor calidad. 