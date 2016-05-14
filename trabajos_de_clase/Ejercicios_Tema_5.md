Servidores Web de Altas Prestaciones.						 Javier Quero Ruiz


###Ejercicios Tema 5

###Ejercicio T5.1. Buscar información sobre cómo calcular el número de conexiones por segundo.

Podemos realizarlo ayudándonos de herramientas que lo calcula por nosotros como por ejemplo nginx o apache benchmark. 
Realizan una serie de peticiones para comprobar el rendimiento del balanceador y a la vez nos dan información sobre él, entre
otras nos pueden indicar el número de conexiones por segundo, el número de fallos, etc.


###Ejercicio T5.3. Buscar información sobre características, disponibilidad para diversos SO, etc de herramientas para monitorizar las prestaciones de un servidor.

Para Windows Server 2008 por ejemplo podemos usar la herramienta Monitor de Confiabilidad y Rendimiento que trae el propio sistema 
operativo. Podemos monitorizar las bases de datos, caché, colas de solicitud de servicio HTTP, la interfaz de red, etc.
Para Ubuntu conocemos más. Por ejemplo la orden top, vmstat o sar por todos conocida. Esta última muy parecida a vmstat.
Su principal uso es para monitorizar la actividad del procesador en tiempo real, tanto la orden top como la orden vmstat tienen 
una interfaz facil de usar y cómoda para obtener la información, aunque no muy vistosa.

He encontrado una nueva herramienta para monitorizar un servidor linux llamada Monitorix. Es más vistosa que las otras tres mencionadas
para este sistema operativo, ya que también ofrece gráficas para ayudar a observar las prestaciones. Da información sobre el tráfico de red,
estadísticas de email, tráfico de servidor web, de carga MySQL, de uso de proxy, etc. 