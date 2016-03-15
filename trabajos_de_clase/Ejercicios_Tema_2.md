Servidores Web de Altas Prestaciones.						 Javier Quero Ruiz


Ejercicios Tema 2


Ejercicio T2.1. Calcular la disponibilidad del sistema si tenemos dos r�plicas de cada elemento (en total 3 elementos en cada subsistema). 

	  
Original			

Web			85%		
Application	90%		
Database	99,9%		
DNS 		98%		
Firewall	85%		
Switch 		99%		
Data Center 99,99%	
ISP 		95%		

Duplicado				

Web			97,75%			
Application	99%			
Database	99,9999%		
DNS 		99,96%		
Firewall	97,75%		
Switch 		99,99%		
Data Center 99,99%		
ISP 		99,75%		

Triplicado

Web			99,66%	
Application	99,9%
Database	99,9999999%
DNS 		99,9992%
Firewall	99,66%
Switch 		99,9999%
Data Center 99,99%
ISP 		99,9875%

La disponibilidad del sistema ser�: 99,198%.


Ejercicio T2.2. Buscar frameworks y librer�as para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad. 

- Strongloop: es el m�s popular framework de c�digo abierto para Node.js.

- Locomotive: es otro framework para Node.js. Tiene integraci�n sin problemas con cualquier motor de base de datos.

- Microsoft Operations Framewotk: framework para productos de Microsoft. 

- Linux-HA: proyecto que mantiene un conjunto de bloques para sistemas de cluster de alta disponibilidad para Linux.


Ejercicio T2.3. �C�mo analizar el nivel de carga de uno de los subsistemas en el servidor? 

Hay muchas herramientas que permiten analizar el nivel de carga de los subsistemas como ya vimos en Ingenier�a de Servidores.  Algunos de ellos son: 

- Top: esta orden nos permite conocer en tiempo real la actividad del sistema.

- Vmstat: que tambi�n puede dar informaci�n sobre accesos a discos y estad�sticas de memoria.

- SysStat: es un paquete de monitorizaci�n.

- Sar: otro monitor que nos permite analizar muy bien el sistema.

- SarCheck: es un programa para an�lisis de prestaciones basado en el monitor sar.

En pr�cticas vimos algunos m�s como Munin, Nagios o Ganglia. 

Ejercicio T2.4. Buscar ejemplos de balanceadores software y hardware (productos comerciales). Buscar productos para servidores de aplicaciones y para servidores de almacenamiento. 

Algunos ejemplos de balanceadores software y hardware son:

- Cisco.

- Barracuda.

- Pound.

- Zen Load. 

Por otra parte algunos productos para servidores de aplicaci�n y de almacenamiento son: 

- WebSphere de IBM.

- WebLogic de Oracle.

- JOnAs.

- De almacenamiento podemos encontrar servidores de HP, de Western Digital o de IBM. Un ejemplo de ellos es el WD Sentinel DX4000.