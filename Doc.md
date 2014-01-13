#<u>Práctica3: Diseño de máquinas virtuales</u>

En esta práctica haremos énfasis al proceso de  diseño de una máquina virtual. Lo importante es que esté adaptada a la tarea a la que se dedica dicha máquina virtual (VM). Desde el punto de vista de sistemas, una máquina debe usar los recursos necesarios para su funcionamiento correcto, y ni uno más, por lo que con la flexibilidad que se tiene para el uso de memoria, número de CPUs y almacenamiento se puede diseñar una máquina con los requisitos necesarios.


## Crear VM en Azure.

La instalación de las máquinas virtuales desde Azure las hemos hecho mediante el panel web. A continuación mostramos detalladamente con capturas de pantalla cómo hemos creado y configurado una máquina virtual en Azure.

Accedemos al panel de control web que Azure nos proporciona
	[https://manage.windowsazure.com/](https://manage.windowsazure.com/)

En la parte inferior izquierda (flecha roja) hacemos Click en Nuevo:

![Creando MV Azure](https://raw2.github.com/rogegg/IV_Practica3/master/imagenes/azure1.png)

Vamos a crear una máquina virtual por lo que seleccionamos *Proceso>Máquina Virtual>De la galería*:

![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure2.png)

Ahora seleccionamos un sistema operativo, en este caso Ubuntu Server 12.04LTS y siguiente:

![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure3.png)

Ahora daremos un nombre a nuestra máquina virtual y al usuario, además podremos seleccionar los núcleos de la misma y cargar una clave ssh o proporcionar una contraseña para conectarnos a la VM.

![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure4.png)



Ya en la página4 configuraremos algunos aspectos como el nombre dns, la región de la red virtual o la cuenta de almacenamiento, como ya creamos una cuenta de almacenamiento en los ejercicios del Tema4, vamos a usarla.
![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure5.png)

Una vez hemos seleccionado la configuración que necesitamos, le indicaremos cómo nos vamos a conectar a la máquina virtual:
![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure5b.png)

> ¡OJO! Como vamos a acceder mas tarde mediante el explorador web, vamos a activar también HTTP.

Le damos a Siguiente y vemos cómo el sistema está creando la máquina virtual.
![Creando MV Azure](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure6.png)



##Conexion a la VM de Azure.

Una vez creada la máquina virtual necesitamos tener acceso a ella para instalar y configurar nuestro sistema para la función que vaya a realizar.

Para conectarnos necesitamos conocer el nombre dns que nos da Azure, para ello vamos al panel de nuestra nueva VM:
![Azure_vm_dns](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure_dns.png)

Una vez conocida la dirección y como la forma de conexión que indicamos fue SSH, desde un terminal de linux:

	$ ssh rogeiv-small.cloudapp.net

![Azure_vm_dns](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/azure_conex.png)

Nos pedirá la contraseña con la que creamos la VM y se conectará.

>¡NOTA! Si hemos cambiado el puerto de conexion puede que tengamos que indicar por qué puerto conectar de la siguiente forma:
	$ ssh rogeiv-small.cloudapp.net:5555




Nuestra aplicación web va a necesitar apache y php, por lo que procedemos a instalarlo.

##1. Apache2 

Para instalar apache de forma sencilla con apt-get:
	$ sudo apt-get install apache2

	$ sudo service apache2 start 


##2. PHP5

Lo que tendremos que hacer es instalar el paquete de php5:

	$ apt-get install php5


¡NOTA! En Ubuntu Server 12.04LTS al intentar instalar php5 necesité incluir el repositorio:
	$ sudo add-apt-repository ppa:ondrej/php5
	$ sudo apt-get update
	$ sudo apt-get upgrade
	$ apt-get install php5


Para comprobar que todo esta bien y php está funcionando podemos crear un pequeño fichero que nos muestre la configuración de php. En la ruta `/var/www/` creamos un fichero `php.info` que contendrá lo siguiente
	
	<?php phpinfo(); ?>

Ahora para comprobar que el servicio apache2 y php están funcionando, y que la máquina virtual está cumpliendo su objetivo que es dar soporte a esta aplicación web accedemos al fichero *info.php* que hemos creado. Para ello debemos obtener la dirección pública que nos da Azure para la VM, la podemos consultar en el panel de control web.

En mi caso la ip pública es 137.117.146.36, luego en el explorador de internet pondremos:

[http://137.117.146.36/info.php](http://137.117.146.36/index.html)

Una vez comprobado que funciona, vamos a enviar los ficheros de la aplicación a la máquina virtual.

###### Ahora ya tenemos todo configurado y preparado para que nuestra aplicación funcione.


##La aplicación.

La aplicación web elegida para alojar en la VM es la que realizamos en la práctica2, con algunos retoques actualizando contenidos y añadiendo funcionalidades de descarga de los materiales de clase.

La aplicación es un índice de los materiales que estamos usando durante la asignatura Infraestructura Virtual.


La aplicación actualmente se encuentra subida y funcionado en Azure en la máquina VM3-CentOS

[http://rogeiv-centoos.cloudapp.net](http://rogeiv-centoos.cloudapp.net)

y en la máquina VM2-Ubuntu-Server

[http://rogeiv.cloudapp.net](http://rogeiv.cloudapp.net/index.php)

##Las máquinas virtuales.

### VM1-Small-Ubuntu-Server -> Montada en Azure
	SO: Ubuntu Server 12.04LTS,		Kernel: 3.2.0-57-virtual
	Núcleos del procesador: 1		RAM: 768MB

### VM2-Ubuntu-Server -> Montada en Azure
	SO: Ubuntu Server 12.04LTS,		Kernel: 3.2.0-57-virtual
	Núcleos del procesador: 1		RAM: 1,75GB

### VM3-CentOS -> Montada en Azure
	SO: CentOS release 6.3 (Final),	Kernel: Linux centoos 2.6.32-279.14.1.el6.openlogic.x86_64
	Núcleos del procesador: 1		RAM: 1,75GB

### VM4-Ubuntu-Server -> Montada con VirtualBox 
	SO: Ubuntu Server 12.04LTS,		Kernel: 3.8.0-29-generic
	Núcleos del procesador: 1		RAM: 1,75GB


##Comparaciones.

Para medir las diferentes máquinas y el tiempo de respuesta de nuestra aplicación hemos usado Apache Benchmark.

	$ ab -n 500 -c 50 <direccion>


De el análisis de apache hemos destacado los siguientes campos:
* Solicitud por segundo (#/s)
* Tiempo de respuesta (ms)
* Velocidad de transferencia (Kbytes/sec)
* Tiempo total por test (s)



###Resultados en tabla y diagramas de barras


![tabla_comparativa](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/tabla_comparativa.jpg)

> Solicitudes por segundo

![solicitud_segundo](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/solicitud_segundo.jpg)

> Tiempo de respuesta

![tiempo_respuesta](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/tiempo_respuesta.jpg)

> Velocidad de transferencia

![velocidad_transferencia](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/velocidad_transferencia.jpg)

> Tiempo por test completo

![tiempo_test](https://raw.github.com/rogegg/IV_Practica3/master/imagenes/tiempo_test.jpg)



##Conclusión

Nuestra aplicación es muy ligera, por lo que no es necesario un aumento excesivo de los recursos. Aunque vemos como la máquina virtual VM2 es más rápida que las demás, tambien debemos tener en cuenta que tiene 1GB más de memoria RAM, por lo que el precio de la máquina es mayor, y esto nos hace plantearnos si merece la pena aumentar 1GB de memoria RAM para apenas ganar 8 solicitudes por segundo. Está claro que para nuestra aplicación con la máquina VM1 que cuenta con aproximadamente 700MB de memoria RAM nos es mas que suficiente.

Vemos como la máquina de CentOs aún teniendo 1,7GB de ram es comparable casi a la máquina de Ubuntu Server con 700MB de ram.




##Repositorio en GitHub
* [https://github.com/rogegg/IV_Practica3](https://github.com/rogegg/IV_Practica3)



##Bibliografía

* [http://askubuntu.com](http://askubuntu.com)
* [http://tuxapuntes.com](http://tuxapuntes.com)
* [www.rickymills.com](www.rickymills.com)
* [Apuntes de la asignatura](http://jj.github.io/IV/)


