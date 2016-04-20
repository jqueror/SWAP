# Pr�ctica 3. Balanceo de Carga.

## Jos� Antonio Larrubia Garc�a
## Javier Quero Ru�z

###1. Nginx.

Lo primero que tenemos que hacer es instalar nginx. En nuestro caso con  *sudo apt-get install nginx* fue suficiente.

Una vez instalado procedemos a realizar la configuraci�n del mismo para que act�e como balanceador de carga.
Como la configuraci�n inicial no nos sirve ya que parte de un servidor web, tenemos que modificar el fichero
*/etc/nginx/conf.d/default.conf*. En nuestro caso el fichero no estaba pero al crearlo no hubo problemas en la
m�quina con ubuntu server 12, en la otra, con ubuntu server 14 no pudimos modificarlo como balanceador. No nos daba error al 
reiniciar el servicio pero no nos balanceaba, s�lo mostraba el index de nginx. Encontramos que era en sites-availables
haciendo un enlace simb�lico hacia sites-enable, pero al hacer esto, daba error al reiniciar el servicio y no 
estaba disponible el puerto 80, por tanto, reinstalamos en una m�quina con ubuntu server 12 y desaparecio el problema.

En la siguiente imagen se puede ver como configuramos nuestro balanceador nginx:

![im1] (/practicas/practica2/capturas/im1.png)

Esta configuraci�n est� hecha antes de a�adir el reparto de carga en el que la m�quina 1 maneja el doble de carga que la 
dos.

Una vez configurado y reiniciado el servicio nginx, cambiamos los index de la m�quina 1 y 2 para que digan quien es cada uno
para saber facilmente cu�l de las dos es la que est� sirviendo y as� saber si funciona correctamente.

Para probarlo hacemos un curl y vemos que nos da el index de cada p�gina, por tanto funciona correctamente.
En la siguiente imagen se puede ver un ejemplo de ejecuci�n.

![im2] (/practicas/practica2/capturas/im2.png)

A continuaci�n ya si asignamos la carga de trabajo que manejar� cada una de las m�quinas, donde suponemos que la m�quina 1
es el doble de potente, o tiene el doble de capacidad que la m�quina 2 (aunque en nuestro caso sean iguales).
En la siguiente imagen se ve como hemos modificado el archivo */etc/nginx/conf.d/default.conf* para que lo haga como nos piden.

![im3] (/practicas/practica2/capturas/im3.png)

Procedemos a probar que realmente asigna el trabajo como debe, es decir, la m�quina 1 sirve el doble de peticiones que la 
la m�quina 2. Para comprobarlo lo hacemos desde otra m�quina, la m�quina 4, que llamar� a la m�quina que act�a de balanceador
con un curl, en nuestro caso quedar�a de la siguiente forma *curl http://192.168.1.105*, ya que esa es la ip de nuestor balanceador. 

En la siguiente imagen observamos que de cada 3 peticiones que llegan la m�quina 2 atiende solamente a una de ellas y la 1 atiende dos.

![im4] (/practicas/practica2/capturas/im4.png)

Ya hemos terminado con nginx, ahora lo haremos con haproxy.

###2. Haproxy.

Lo primero que tenemos que hacer es matar nginx con la orden: *service nginx stop* en modo superusuario(root).

Una vez detenido el servicio de nginx procedemos a instalar haproxy. El orden de esto da igual, lo importante es
que cuando queramos usar haproxy, nginx est� parado y haproxy iniciado, sino habr� un conflicto a la hora de 
escuchar por el puerto 80. Para instalar haproxy usamos apt-get como anteriormente.

Una vez instalado debemos modificar su archivo de configuraci�n, en este caso lo podemos encontrar en */etc/haproxy/haproxy.cfg*.
Esta vez ya hemos a�adido que asigne la carga adecuadamente desde el principio.

En la siguiente imagen vemos como queda el fichero de configuraci�n.

![im5] (/practicas/practica2/capturas/im5.png)

Una vez tenemos el fichero de configuraci�n modificado, lanzamos haproxy. Para ello usamos la siguiente orden:
*/usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg*. Y desde la m�quina 4 llamamos de nuevo al balanceador de la misma forma que hicimos
con nginx para comprobar que lo hace correctamente. En la siguiente imagen puede verse un ejemplo de como funciona.

![im6] (/practicas/practica2/capturas/im6.png)

Como se observa tambi�n funciona correctamente.

###3. Pound.

De nuevo instalamos pound como en las dos ocasiones anteriores y matamos nginx y haproxy si est�n activos.
Editamos el archivo de configuraci�n */etc/pound/pound.cfg* para que realice lo pedido en la pr�ctica, tambi�n los pesos.

![im7] (/practicas/practica2/capturas/im7.png) 
		
Quedando como mostramos anteriormente. Despu�s editamos el archivo */etc/default/pound* cambiando startup a 1 para poder arrancarlo.

![im8] (/practicas/practica2/capturas/im8.png)
		
Para que inicie introducimos la orden */etc/init.d/pound start*.
Y por �ltimo llamamos al balanceador desde la m�quina 4 como hemos hecho con las otras dos configuraciones.

![im9] (/practicas/practica2/capturas/im9.png)
		
Observamos en la imagen anterior que funciona como es debido. 
		