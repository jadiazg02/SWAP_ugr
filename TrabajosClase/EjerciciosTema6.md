#Ejercicio T6.2:

##Comprobar qué puertos tienen abiertos nuestras máquinas, su estado, y qué programa o demonio lo ocupa.

Para ver los puertos abiertos y las aplicaciones en mac podemos usar lo siguiente:

    lsof -i -P | grep -i listen

Lo que nos ofrece el siguiente resultado:

SilentCle  319 joseadiazg   10u  IPv4 0xd4ba2349429bddad      0t0  TCP localhost:6666 (LISTEN)
Dropbox    352 joseadiazg   66u  IPv6 0xd4ba2349494e883d      0t0  TCP *:17500 (LISTEN)
Dropbox    352 joseadiazg   69u  IPv4 0xd4ba23494b2828c5      0t0  TCP *:17500 (LISTEN)
Dropbox    352 joseadiazg   92u  IPv4 0xd4ba23494981ddad      0t0  TCP localhost:17600 (LISTEN)
Dropbox    352 joseadiazg   97u  IPv4 0xd4ba234948f978c5      0t0  TCP localhost:17603 (LISTEN)
SpotifyWe  374 joseadiazg    6u  IPv4 0xd4ba2349429befbd      0t0  TCP localhost:4370 (LISTEN)
SpotifyWe  374 joseadiazg    7u  IPv4 0xd4ba2349429bf8c5      0t0  TCP localhost:4380 (LISTEN)
Adobe\x20  415 joseadiazg   15u  IPv4 0xd4ba2349429be6b5      0t0  TCP localhost:15292 (LISTEN)


#Ejercicio T6.3: 

##Buscar información acerca de los tipos de ataques más comunes en servidores web, en qué consisten, y cómo se pueden evitar.

Inyeccion sql: Es un ataque muy común en servidores, páginas web o sistemas de gestión de contenidos vulnerables. Consisnte en introducir en la base de datos, consultas SQL donde no deberia de haberlas pudiendo obtener datos de la base de datos o cambiar informacion como por ejemplo el sistema de codificacion a UTF-7 por ejemplo que si bien la solución es rápida, asusta bastante cuando entras a tu web y te la encuentras en "portugues". La solucion a este tipo de ataques pasa por usar validaciones en todos los formularios o sistemas de comentarios que volcaran contenido en la base de datos subyacente. 

Fuerza bruta: Es una manera muy sencilla de obtener por ejemplo una contraseña de un sistema web... buscando alguna vulnerabilidad o plugin por actualizar podemos obtener mediante enlaces las datos como por ejemplo un usuario, por lo cual ya tendremos el 50% de lo que necesitamos apra el acceso, tras ello usando un archivo y un sencillo script de fuerza bruta podremos reventar  una contraseña en poco tiempo. La solución es sencilla, contraseñas de seguridad elevada, cambios constantes y software actualizado. 