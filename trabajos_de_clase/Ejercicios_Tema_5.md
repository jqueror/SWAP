Servidores Web de Altas Prestaciones.						 Javier Quero Ruiz


###Ejercicios Tema 5

###Ejercicio T5.1. Buscar informaci�n sobre c�mo calcular el n�mero de conexiones por segundo.

Podemos realizarlo ayud�ndonos de herramientas que lo calcula por nosotros como por ejemplo nginx o apache benchmark. 
Realizan una serie de peticiones para comprobar el rendimiento del balanceador y a la vez nos dan informaci�n sobre �l, entre
otras nos pueden indicar el n�mero de conexiones por segundo, el n�mero de fallos, etc.


###Ejercicio T5.3. Buscar informaci�n sobre caracter�sticas, disponibilidad para diversos SO, etc de herramientas para monitorizar las prestaciones de un servidor.

Para Windows Server 2008 por ejemplo podemos usar la herramienta Monitor de Confiabilidad y Rendimiento que trae el propio sistema 
operativo. Podemos monitorizar las bases de datos, cach�, colas de solicitud de servicio HTTP, la interfaz de red, etc.
Para Ubuntu conocemos m�s. Por ejemplo la orden top, vmstat o sar por todos conocida. Esta �ltima muy parecida a vmstat.
Su principal uso es para monitorizar la actividad del procesador en tiempo real, tanto la orden top como la orden vmstat tienen 
una interfaz facil de usar y c�moda para obtener la informaci�n, aunque no muy vistosa.

He encontrado una nueva herramienta para monitorizar un servidor linux llamada Monitorix. Es m�s vistosa que las otras tres mencionadas
para este sistema operativo, ya que tambi�n ofrece gr�ficas para ayudar a observar las prestaciones. Da informaci�n sobre el tr�fico de red,
estad�sticas de email, tr�fico de servidor web, de carga MySQL, de uso de proxy, etc. 