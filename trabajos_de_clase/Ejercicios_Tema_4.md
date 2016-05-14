Servidores Web de Altas Prestaciones.						 Javier Quero Ruiz


###Ejercicios Tema 4

###Ejercicio T4.1. Buscar informaci�n sobre cu�nto costar�a en la actualidad un mainframe. Comparar precio y potencia entre esa
m�quina y una granja web de unas prestaciones similares. 
  
He buscado informaci�n sobre el mainframe de IBM z13, sacado por IBM al principio de 2015. Es el primer mainframe capaz de procesar 
2500 millones de transacciones al d�a, no han revelado el precio, aunque sus antecesores estaban entre 75.000$ y 2m$. Seg�n algunos 
foros puede seguro que supera los 100.000$.
Los precios de las granjas web son m�s asequibles, pero no he podido comparar ninguna con este mainframe ya que es elevado a cualquier
granja web del mercado. No he llegado a encontrar nada similar.


###Ejercicio T4.6. Buscar informaci�n sobre los bloques de IP para los distintos pa�ses o continentes. 
Implementar en JavaScript o PHP la detecci�n de la zona desde donde se conecta un usuario.

En esta pagina (https://www.countryipblocks.net/allocation-of-ip-addresses-by-country.php) tenemos los 246 paises con sus 
n�meros de IPs asignadas para cada uno. Hay un total de 4,294,967,296 direcciones IP en el mundo. 
Para implementar la detecci�n de la zona desde donde se conecta un usuario necesitamos una base de datos, que la podemos descargar 
de aqu�: http://chir.ag/projects/geoiploc/ y con un script que haga uso de ella podemos detectar de que pa�s proviene esa direcci�n IP.

<?php
error_reporting(E_ALL & ~E_NOTICE);
include("geoiploc.php"); 
  if (empty($_POST['checkip']))
  {
        $ip = $_SERVER["REMOTE_ADDR"]; 
  }
  else
  {
        $ip = $_POST['checkip']; 
  }
?> 
Tu direcci�n IP es: <?php echo($ip); ?> <br>
Tu Pa�s es : <?php echo(getCountryFromIP($ip, " NamE"));?>
 (<?php echo(getCountryFromIP($ip, "code"));?>)

***No lo he realizado yo, est� sacado de la p�gina web: https://norfipc.com/codigos/como-saber-pais-corresponde-direccion-ip-internet.php



 