# Pr�ctica 6. Discos en RAID.

## Jos� Antonio Larrubia Garc�a
## Javier Quero Ru�z

###1. Configuraci�n del RAID por software.
Con la m�quina apagada le hemos a�adimos dos discos de un 1Gb cada uno, llamados disco1 y disco2.

![im1] (/practicas/practica6/capturas/im1.png)

Despu�s instalamos mdadm para poder configurar RAID. Lo hacemos con la orden *apt-get install mdadm*.
Buscamos la informaci�n de ambos discos con fdisk -l.

![im2] (/practicas/practica6/capturas/im2.png)

Creamos el RAID1 usando el dispositivo /dev/md0.

![im3] (/practicas/practica6/capturas/im3.png)

Le damos formato con *mkfs /dev/md0*.

![im4] (/practicas/practica6/capturas/im4.png)

Creamos el directorio en el que se montar� RAID con las ordenes *mkdir /dat* y *mount /dev/md0 /dat*.
Una vez hecho esto procedemos a comprobar el estado del RAID.

![im5] (/practicas/practica6/capturas/im5.png)

Por �litmo configuramos el sistema para que monte el RAID al arrancar el sistema. Para ello debemos editar el archivo
/etc/fstab.
Antes obtenemos el UUID del RAID, ya que lo necesitaremos.

![im6] (/practicas/practica6/capturas/im6.png)

Editamos /etc/fstab quedando como se muestra en la siguiente captura.

![im7] (/practicas/practica6/capturas/im7.png)


###2. Simulaci�n de error en un disco.

Simulamos el fallo.

![im8] (/practicas/practica6/capturas/im8.png)

Comprobamos que se ha producido el fallo.

![im9] (/practicas/practica6/capturas/im9.png)

En la imagen anterior podemos ver que se ha producido el fallo (faulty spare).
Ahora procedemos a retirar en caliente el disco que falla.

![im10] (/practicas/practica6/capturas/im10.png)

En la captura anterior vemos como se ha retirado el disco que fallaba.
Y por �ltimo podemos volver a a�adir un nuevo disco que reemplazar� al disco que hemos retirado.

![im11] (/practicas/practica6/capturas/im11.png)

###3. Configaraci�n NFS.

Primero lo instalamos en la m�quina en la que tenemos el RAID con la orden *apt-get install fs-kernel-server nfs-common rpcbind*.
Comprobamos que se ha instalado perfectamente con grep. 

![im12] (/practicas/practica6/capturas/im12.png)

Configuramos el NFS. 
Creamos una carpeta con la orden *mkdir /compartido*.
Cambiaremos el nombre del usuario y grupo propietarios de la carpeta, para que no sean propiedad de nadie, y tambi�n
los permisos de acceso, para que todos los usuarios dispongan de todos los permisos sobre ella con las ordenes *chown nobody:nogroup /compartido* y
*chmod -R 777 /compartido*, como se muestra a continuaci�n.

![im13] (/practicas/practica6/capturas/im13.png)

Despu�s de esto, debemos editar el archivo */etc/exports*. Este es el archivo donde se indican a NFS las carpetas que vamos a compartir. 
Y reiniciamos el servicio.

![im14] (/practicas/practica6/capturas/im14.png)

En */etc/exports* se pone la ruta del fichero con el cliente y las opciones, al poner * es que cualquier
cliente podr� acceder a esa carpeta compartida, entre parentesis van las opciones.

Por ejemplo:
rw, es lectura escritura.
sync, evita responder peticiones antes de escribir los cambios pendientes en disco.
no_root_squash, deshabilita root squash que evita que los usuarios con privilegios los mantengan sobre la carpeta compartida.
no_subtree_check, deshabilita subtree_check que hace que cuando el directorio compartido es un subdirectorio mayor nfs compruebe los directorios
por encima, para verificar sus permisos y caracter�sticas.

Despu�s de hacer todo esto nos vamos al cliente.
He instalamos *apt-get install nfs-common rpcbind*.

Creamos un punto de montaje para las carpetas compartidas con *sudo mkdir -p /mnt/nfs/home* y *sudo mkdir -p /mnt/nfs/compartido*, que son nuestras dos carpeta que vamos a compartir.

![im15] (/practicas/practica6/capturas/im15.png)

Realizamos el montaje de las carpetas compartidas y con *df -h* probamos que se haya montado bien.

![im16] (/practicas/practica6/capturas/im16.png)

Podemos observar en la imagen anterior que se han montado correctamente.
Ya tenemos configurado NFS, para probarlo creamos ficheros y carpetas dentro de la compartida y vemos que en la otra m�quina tambi�n aparecen.

![im17] (/practicas/practica6/capturas/im17.png)
![im18] (/practicas/practica6/capturas/im18.png)

Para la parte de NFS nos hemos basado en el siguiente tutorial:
http://somebooks.es/?p=5598
