Servidores Web de Altas Prestaciones.						 Javier Quero Ruiz


###Ejercicios Tema 3

###Ejercicio T3.1. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el
enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra. 
  
Bajo Linux lo podemos configurar con la orden iptables y con el comando route. Toda la información de la configuración de red 
se encuentra en */etc/network/interfaces*.
En Windows podemos configurarlo en el panel de control > Entorno de red.


###Ejercicio T3.2. Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado
y bloqueo de paquetes.

Bajo Linux se hace a través de iptables y en Windows se puede hacer a escbribiendo wf.msc por el terminal, que nos llevará 
al firewall de windows con seguiridad avanzada, donde podremos cambiar los valore que deseemos. 
