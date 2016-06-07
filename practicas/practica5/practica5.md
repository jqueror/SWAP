# Pr�ctica 5. Replicaci�n de bases de datos MySQL.

## Jos� Antonio Larrubia Garc�a
## Javier Quero Ru�z

###1. Crear la base de datos.

Lo primero que debemos hacer es crear una base de datos e insertar datos en ella.
Creamos los datos en la m�quina 1. 

Usamos *mysql -uroot -p para entrar en MySQL*.
Creamos la base de datos contactos como nos explican en el pdf.

![im1] (/practicas/practica5/capturas/im1.png)

Ya tenemos creada la base de datos con datos insertados.

###2. Replicar usando Mysqldum.

Ahora vamos a proceder a replicarla con la herramienta mysqldump.
Pero antes de hacer la copia de seguridad tenemos que evitar que se acceda a la base de datos para 
que no se cambie nada mientras hacemos las operaciones, ya que puede estar actualiz�ndose constantemente en el servidor principal (m�quina 1),
lo haremos con la orden *FLUSH TABLES WITH READ LOCK;* como vemos en la siguiente imagen.

![im2] (/practicas/practica5/capturas/im2.png)

Una vez hecho esto podemos hacer el mysqldump para guardar los datos.
En el servidor principal introducimos la orden *mysqldump contactos -u root -p > /root/contactos.sql*
Como hab�amos bloqueado las tablas debemos desbloquearlas, lo haremos con *UNLOCK TABLES;*.

![im3] (/practicas/practica5/capturas/im3.png)

Ya podemos ir a la m�quina esclava que es nuestra m�quina 2 para copiar el archivo .SQL con la orden
*scp root@192.168.1.101:/root/contactos.sql /root/*.

![im4] (/practicas/practica5/capturas/im4.png)

Una vez hecho esto ya tenemos copiada en la m�quina 2 los datos de la base de datos de la m�quina 1.
Para ver que todo ha funcionado comprobamos que esta todo correctamente en la m�quina 2. En este caso es ver que se ha replicado la base de datos creada en la m�quina 1.

![im5] (/practicas/practica5/capturas/im5.png)

Como podemos comprobar, tenemos la base de datos en la m�quina 2.

###3. Replicar maestro-esclavo automaticamente.

Haremos lo mismo que anteriormente solo que lo haremos para poder automatizar el proceso.
Primero configuraremos el maestro, para ello editamos */etc/mysql/my.cnf*.
Comentamos *#bind-address 127.0.0.1* y descomentamos (aparec�a comentado en nuestro caso) *server-id = 1* y *log_bin...*.
Guardamos y reinicimamos el servicio.

Una vez configurado el maestro pasamos al esclavo (nuestra m�quina 2).

![im6] (/practicas/practica5/capturas/im6.png)

Volvemos al maestro para crear un usuario y darle permisos de acceso para la replicaci�n.

![im7] (/practicas/practica5/capturas/im7.png)

![im8] (/practicas/practica5/capturas/im8.png)

Volvemos al esclavo (m�quina 2) y le damos los datos del maestro en mysql.

![im9] (/practicas/practica5/capturas/im9.png)

Ahora vamos a hacer pruebas en el maestro para ver si se replica autom�ticamente en el esclavo.
Volvemos a activar las tablas en el maestro.
En el esclavo comprobamos que el valor de la variable *"Seconds_Behind_Master"* es distinto de *"null"*.

![im10] (/practicas/practica5/capturas/im10.png)

Como podemos ver es distinto de null, ya que tiene un 0, por tanto, deber�a funcionar correctamente.
Por �ltimo vamos a realizar un prueba comprobando que realmente funciona como debe.

En los ejemplos de la imagen siguiente podemos ver como todo funciona correctamente.

![im11] (/practicas/practica5/capturas/im11.png)

###4. Realizaci�n de configuraci�n maestro-maestro.

Haremos lo mismo pero hacia al otro lado, es decir, haciendo la m�quina 2 maestro y la m�quina 1 esclavo para que as� 
haya una comunicaci�n doble, o lo que es lo mismo una comunicaci�n maestro-maestro.

En la m�quina 2:

![im12] (/practicas/practica5/capturas/im12.png)

En la m�quina 1:

![im13] (/practicas/practica5/capturas/im13.png)

Lo hacemos con la ip del maestro. En este caso el de la m�quina 2 y ya estamos listos para volver a 
comprobar el valor de *"Seconds_Behind_Master"* y ver que de nuevo vuelve a ser distinto de *"null"*, vuelve a ser 0.

![im14] (/practicas/practica5/capturas/im14.png)

Por tanto deber�a funcionar.
Comprobamos que funciona bien:

![im15] (/practicas/practica5/capturas/im15.png)

Como vemos podemos insertar nuevos datos tanto en uno como en otro y se replicar� automaticamente en la
m�quina en la que no se haya insertado directamente el dato.