# Pr�ctica 4. Comprobar el rendimiento de servidores web.

## Jos� Antonio Larrubia Garc�a
## Javier Quero Ru�z

###1. Apache Benchmark.

Lo primero que hay que hacer es crear la p�gina web en formato html para que las pruebas que se hagan
tengan suficiente carga y as� los resultados sean representativos.
En nuestro caso hemos usado directamente el ejemplo que viene en el gui�n en */var/www* tanto en la 
m�quina 1 como en la m�quina 2. Uno de los errores que cometimos inicialmente fue olvidarnos de poner 
el html en la m�quina 2 y por tanto la cantidad de fallos al acceder era enorme. Posteriormente en el apartado de siege 
nos dimos cuenta que los dos archivos de ambas m�quinas no los habiamos creado totalmente iguales, por lo que tambi�n nos daban errores.
En la siguiente imagen vemos el ejemplo usado: 

![im1] (/practicas/practica4/capturas/im1.png)

Ahora vamos a mostrar una tabla con las medidas desde la m�quina 4 a la 1, es decir, directamente al servidor
sin pasar por el balanceador. Para ello lanzamos *lanzamos ab -n 1000 -c 5 http://192.168.1.101/prueba.html*.

|M1         | Time taken per tests (s) | Failed requests | Requests per second |
|:---------:|:------------------------:|:---------------:|:-------------------:|
|1          |0,374                     |0                |2670	               | 
|2          |0,362                     |0                |2761	               |
|3          |0,367                     |0                |2722	               |
|4          |0,359                     |0                |2782	               |
|5          |0,371                     |0                |2694	               |
|**Media**  |**0,3666**                |**0**            |**2725,8**           |

En la siguiente imagen vemos un ejemplo de ejecuci�n de la orden ab.

![im2] (/practicas/practica4/capturas/im2.png)

Las siguientes medidas son desde la m�quina 4 a la granja web usando nginx como balanceador. Por tanto en este caso la
orden a usar ser� la misma pero usando la ip de nuestro balanceador *ab -n 1000 -c 5 http://192.168.1.105/prueba.html*.

|Nginx      | Time taken per tests (s) | Failed requests | Requests per second |
|:---------:|:------------------------:|:---------------:|:-------------------:|
|1          |0,694                     |0                |1441	               |
|2          |0,759                     |0                |1318	               |
|3          |0,679                     |0                |1472	               |
|4          |0,679                     |0                |1472	               |
|5          |0,677                     |0                |1476	               |
|**Media**  |**0,6976**                |**0**            |**1435,8**           |

Sin hacer una gr�fica podemos ver ya directamente que si se hicieran peticiones directamente al servidor en lugar de a la 
granja web pasando por el balanceador el servicio es m�s r�pido, algo previsible. 

En la siguiente imagen se puede ver la primera ejecuci�n de ab.

![im3] (/practicas/practica4/capturas/im3.png)

Las siguientes medidas son al igual que las anteriores desde la m�quina 4 hacia la granja web, en este caso usando haproxy
como balanceador. La orden ab sigue siendo la misma que antes.

|Haproxy    | Time taken per tests (s) | Failed requests | Requests per second |
|:---------:|:------------------------:|:---------------:|:-------------------:|
|1          |0,541                     |0                |1847	               |
|2          |0,541                     |0                |1847	               |
|3          |0,539                     |0                |1855	               |
|4          |0,549                     |0                |1820	               |
|5          |0,528                     |0                |1892	               |
|**Media**  |**0,5396**                |**0**            |**1852,2**           |

De nuevo s�lo con los datos de la tabla podemos ver como haproxy hace un mejor servicio de balanceador que nginx, nada raro
considerando que nginx en un principio es software para servidor web y no como haproxy que si es un balanceador directamente.
A�n as� sigue siendo m�s r�pido accediendo al servidor directamente.

Para este caso no hemos hecho captura ya que la salida es similar en todos los casos de ab.

Por �ltimo las medidas con pound como balanceador desde la m�quina 4.

|Pound      | Time taken per tests (s) | Failed requests | Requests per second |
|:---------:|:------------------------:|:---------------:|:-------------------:|
|1          |1,051                     |0                |951	               |
|2          |1,034                     |0                |967	               |
|3          |1,046                     |0                |956	               |
|4          |1,043                     |0                |958	               |
|5          |1,033                     |0                |968	               |
|**Media**  |**1,0414**                |**0**            |**960**              |

Podemos decir que pound es de los tres balanceadores usados en la pr�ctica el m�s lento.

En la siguiente imagen mostramos las gr�ficas donde se ven comparadas las medias de time, taken per tests y request per second de las tres
tablas anteriores.

![gra1] /practicas/practica4/capturas/gra1.png)

En esta primera gr�fica no hemos mostrado failed request por ser siempre 0 en todas las mediciones.

###2. Siege.

Lo primero es instalar siege con la orden *apt-get install siege*.
Una vez instalado repetiremos las medidas que se han realizado con ab.

Las primeras como antes ser�n desde la m�quina 4 hacia la m�quina 1 directamente. La orden usada de siege es 
*siege -b -t60s -v http://192.168.1.101/prueba.html*, en la siguiente imagen se ve una primera ejecuci�n de siege.

![im4] (/practicas/practica4/capturas/im4.png)

Y ahora la tabla con las mediciones.

|M1         | Availability(%) | Reponse time (s) | Failed transactions | Longest transaction |
|:---------:|:---------------:|:----------------:|:-------------------:|:-------------------:|
|1          |100              |0,01              |0	                   |0,15                 |
|2          |100              |0,01              |0	                   |0,1                  |
|3          |100              |0,01              |0	                   |0,17                 |
|4          |100              |0,01              |0	                   |0,06                 |
|5          |100              |0,01              |0	                   |0,16                 |
|**Media**  |**100**          |**0,01**          |**0**                |**0,128**            |

Las siguientes mediciones son desde la m�quina 4 hacia la granja web con nginx como balanceador. La orden de siege
por tanto queda de la siguiente forma *siege -b -t60s -v http://192.168.1.105/prueba.html*.

|Nginx      | Availability(%) | Reponse time (s) | Failed transactions | Longest transaction |
|:---------:|:---------------:|:----------------:|:-------------------:|:-------------------:|
|1          |100              |0,01              |0	                   |0,06                 |
|2          |100              |0,01              |0	                   |0,05                 |
|3          |100              |0,01              |0	                   |0,07                 |
|4          |100              |0,01              |0	                   |0,06                 |
|5          |100              |0,01              |0	                   |0,05                 |
|**Media**  |**100**          |**0,01**          |**0**                |**0,058**            |

En este caso la �nica diferencia es en la transacci�n m�s larga.

Las siguientes son con haproxy como balanceador.

|Haproxy    | Availability(%) | Reponse time (s) | Failed transactions | Longest transaction |
|:---------:|:---------------:|:----------------:|:-------------------:|:-------------------:|
|1          |100              |0,01              |0	                   |0,05                 |
|2          |100              |0,01              |0	                   |0,06                 |
|3          |100              |0,01              |0	                   |0,05                 |
|4          |100              |0,01              |0	                   |0,06                 |
|5          |100              |0,01              |0	                   |0,06                 |
|**Media**  |**100**          |**0,01**          |**0**                |**0,056**            |

Y por �ltimo las medidas usando pound.

|Pound      | Availability(%) | Reponse time (s) | Failed transactions | Longest transaction |
|:---------:|:---------------:|:----------------:|:-------------------:|:-------------------:|
|1          |100              |0,01              |0	                   |0,1                  |
|2          |100              |0,01              |0	                   |0,11                 |
|3          |100              |0,01              |0	                   |0,15                 |
|4          |100              |0,01              |0	                   |0,15                 |
|5          |100              |0,01              |0	                   |0,16                 |
|**Media**  |**100**          |**0,01**          |**0**                |**0,134**            |

La gr�fica solo corresponde a las diferencias entre las medias de las transacciones m�s largas de todos los casos ya que
todas las otras medidas son id�nticas.

![gra2] (/practicas/practica4/capturas/gra2.png)

###3. Httperf.

De nuevo como las otras dos veces primeramente debemos instalarlo. Podemos de nuevo hacerlo usando el repertorio de paquetes, en este caso la orden ser�a
*apt-get install httperf*.

Para las primeras mediciones de httperf lo haremos desde la m�quina 4 a la m�quina 1 directamente como anteriormente. Se usa la orden 
*httperf --hog --server 192.168.1.101 http://192.168.1.101/prueba.html --port80 --num-conns 1000* donde se especifica el 
server al que le haces la petici�n y donde est� el archivo, el puerto donde debe mirar y el n�mero de peticiones. En nuestro
caso hemos puesto 1000 ya que era lo que se usaba en apache benchmark.

La siguiente imagen muestra un ejemplo de ejecuci�n.

![im5] (/practicas/practica4/capturas/im5.png)

A continuaci�n en la tabla mostramos las cinco medidas.

|M1         | Connection rate(conns/s)| Avg connection time(ms) | Reply time (ms) |
|:---------:|:-----------------------:|:-----------------------:|:---------------:|
|1          |715,8                    |1,4                      |0,7              |
|2          |647,5                    |1,5                      |0,8              |
|3          |739                      |1,4                      |0,7              |
|4          |648,9                    |1,5                      |0,8              |
|5          |666,1                    |1,5                      |0,8              |
|**Media**  |**683,46**               |**1,46**                 |**0,76**         |

Ahora lo ejecutaremos desde la m�quina 4 a la granja web con nginx como balanceador. La orden de httperf en este caso ser�a la siguiente:
*httperf --hog --server 192.168.1.105 http://192.168.1.105/prueba.html --port80 --num-conns 1000*

|Nginx      | Connection rate(conns/s)| Avg connection time(ms) | Reply time (ms) |
|:---------:|:-----------------------:|:-----------------------:|:---------------:|
|1          |485,5                    |2,1                      |1,4              |
|2          |502,2                    |2                        |1,3              |
|3          |494,4                    |2                        |1,4              |
|4          |507,2                    |2                        |1,3              |
|5          |483,1                    |2,1                      |1,4              |
|**Media**  |**494,48**               |**2,04**                 |**1,36**         |
		
Las siguientes medidas ser�n con haproxy como balanceador.

|Haproxy      | Connection rate(conns/s)| Avg connection time(ms) | Reply time (ms) |
|:---------:|:-----------------------:|:-----------------------:|:---------------:|
|1          |546,2                    |1,8                      |1,1              |
|2          |553                      |1,8                      |1,1              |
|3          |556,1                    |1,8                      |1,1              |
|4          |570,8                    |1,8                      |1,1              |
|5          |550,5                    |1,8                      |1,1              |
|**Media**  |**555,32**               |**1,8**                  |**1,1**          |

Y por �ltimo las medidas con pound como balanceador.

|Pound      | Connection rate(conns/s)| Avg connection time(ms) | Reply time (ms) |
|:---------:|:-----------------------:|:-----------------------:|:---------------:|
|1          |428,5                    |2,3                      |1,6              |
|2          |448                      |2,2                      |1,5              |
|3          |467,2                    |2,1                      |1,4              |
|4          |443                      |2,3                      |1,5              |
|5          |446                      |2,2                      |1,4              |
|**Media**  |**446,54**               |**2,22**                 |**1,48**          |

En la siguiente imagen podemos ver las 3 gr�ficas comparando las medias de cada una de las tablas.

![gra3] (/practicas/practica4/capturas/gra3.png)

Podemos por tanto decir que cada benchmark eval�a de forma distinta al servidor web dando distintos tipos de 
rendimiento, as� por tanto, podremos poner a prueba a nuestro server con un benchmark donde se de informaci�n concreta
sobre algo que estemos buscando. Es decir, si queremos probar la transacci�n m�s larga tenemos siege, si por ejemplo queremos ver 
las peticiones que sirve por segundo tenemos apache o si quisieramos saber el tiempo de respuesta httperf.

Una buena forma de evaluar nuestro server y en este caso elegir balanceador seg�n lo que estemos buscando.




