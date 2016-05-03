#Práctica 4: SWAP 

En esta práctica primero vamos a realizar una bateria de pruebas contra una de las máquinas por separado con Apache Benchmark y Siege. Tras esto, realizaremos las mismas pruebas contra la granja web con el balanceo mediante nginx o haproxy. 


##Pruebas usando Apache Benchmark:

El comando usado ha sido el siguiente:

    ab -n 10000 -c 5 http://192.168.56.4/pract1.html 


Los resultados podemos verlos en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/ab1.png "Tablas parametros AB.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/ab2.png "Gráficas parametros AB.")

##Pruebas usando Siege:

El comando usado ha sido el siguiente:

    siege -b -t10S -v 192.168.56.4/pract1.html 


Los resultados podemos verlos en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/siege1.png "Tablas parametros Siege.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/siege2.png "Gráficas parametros Siege.")


##Pruebas usando HTPERF:

Como entrega opcional se han realizado tambien una bateria de pruebas usando HTPERF. El comando utilizado ha sido el siguiente:

httperf --server 192.168.56.4 --port 80 --uri /pract1.html --rate 150 --num-conn 1000 --num-call 1 --timeout 5


Los resultados podemos verlos en la siguiente imagen. 

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/htperf1.png "Tablas parametros Htperf.")

![Con titulo](https://github.com/joseangeldiazg/SWAP_ugr/blob/master/pantallazosSWAP4/htperf2.png "Gráficas parametros Htperf.")

