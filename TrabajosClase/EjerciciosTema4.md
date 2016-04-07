#Ejercicio T4.1:

##Buscar información sobre cuánto costaría en la actualidad un mainframe. Comparar precio y potencia entre esa máquina y una granja web de unas prestaciones similares.

Aunque pueden parecer obsoletos, empresas como IBM sigue ofreciendo a sus clientes mainframes. Uno en concreto de "bajo coste" es el modelo zEnterprise 114 que podemos encontrar por el precio de 410000$ e incluye tres años de garantia y SW de virtualización. Algunas de sus caracteristicas que hemos obtenido de su web son:

-Escalabilidad vertical: más de 3.100 millones de instrucciones por segundo (MIPS) de propósito general en un único espacio.
-Escalabilidad horizontal: consolidación de hasta 300 servidores distribuidos.4
-Escalabilidad interna: procesadores especializados, procesadores criptográficos, hipervisores.
-Escalabilidad más allá de los límites tradicionales: si se configura con el zBX, admite la integración de hasta 112 servidores blade distribuidos y optimizadores de cargas de trabajo.

Aunque nos lo presentan como una posible solucion para empresas no muy grandes, o pymes, la verdad es que para una de estas empresas una solución mucho mejor y economica seria uan granja web o simplemente contratar alguna plataforma online de Cloud Computing. 

#Ejercicio T4.2: 

##Buscar información sobre precio y características de balanceadores hardware específicos. Compara las prestaciones que ofrecen unos y otros.

Vamos a comparar los balanceadores: KEMP LM-8000 y el F5 Networks LTM-2200s. 

Característica| KEMP LM-8000 | F5 Networks LTM-2200s
--------------|--------------|----------------------
Precio        | 28000$       | 21920$
Garantia Hardware| 1 año | 1 año
Garantia Software| 1 año | 90 días
Ancho de banda en GB| 20 | 5
Compresion HTTP | Si | Software
Número conexiones a corriente | 2| 1
Memoria Standar | 64GB| 8GB


#Ejercicio T4.3

##Buscar información sobre los métodos de balanceo que implementan los dispositivos recogidos en el ejercicio 4.2.

El KEMP LM-800 tal y como podemos ver en su página web, implementa entre otros los siguientes métodos de balanceo: 

Round Robin
Weighted Round Robin
SDN Adaptive
Least Connection
Weighted Least Connection


#Ejercicio T4.6:
##Buscar información sobre los bloques de IP para los distintos países o continentes. Implementar en JavaScript o PHP la detección de la zona desde donde se conecta un usuario

En la web http://chir.ag/projects/geoiploc/ podemos encontrar un extenso material sobre los rangos de ip de cada pais, contiente e incluso por criterios más precisos. Ademas mantienen una lista con los bloques de ip de cada pais actualizada continuamente muy útil por si necesitas banear ciertos ip o paises en tu web. Un Script sencillo para obtener las IP podria ser, el cual hemos obtenido como referencia de la siguiente web (http://blogandweb.com/php/detectar-el-pais-de-referencia-de-nuestros-usuarios-con-php/):

    $_SERVER["HTTP_CLIENT_IP"]!=""?$ip=$_SERVER["HTTP_CLIENT_IP"]:
    $ip=$_SERVER["REMOTE_ADDR"];
    //Función de obtención de IP (basado en la web de webhosting.info)
    function getCountry($ip_address)
    {
        //By Marc Palau (http://www.nbsp.es)
        $url = "http://ip-to-country.webhosting.info/node/view/36";

        $inici = "src=/flag/?type=2&cc2=";

        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST,"POST");
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_POSTFIELDS, "ip_address=$ip_address"); 

        ob_start();

        curl_exec($ch);
        curl_close($ch);    
        $cache = ob_get_contents();
        ob_end_clean();

        $resto = strstr($cache,$inici);
        $pais = substr($resto,strlen($inici),2);

        return $pais;
    }
    //obtención de código de país:
    $pais = strtolower(getCountry($ip));


