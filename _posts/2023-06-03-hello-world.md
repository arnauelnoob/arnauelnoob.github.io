---
title: "Hello World"
date: 2023-06-03 00:00:00 +0800
categories: [Hello World]
tags: [Hello World]
---

# Hello World

Hello World this my personal blog.

![[Pasted image 20250530175911.png]]


Nos encontramos con una maquina literalmente como la Granny, voy a poner la misma información:


Nos encontramos con lo siguiente en nuestra maquina victima:

![[Pasted image 20250529113150.png]]

Unicamente tenemos el puerto 80, despues de intentar buscar una vulnerabilidad web a traves de metodos como fuzzing y tal, no veo posibilidad de hacer nada, por lo que voy a buscar la version en internet a ver sie ncuentro algo **iis 6.0**

![[Pasted image 20250529113327.png]]

Me encuentro el siguiente **cve: 2017-7269** tras buscarlo en metasploit, nos encontramos con el siguiente explioit:

![[Pasted image 20250529113527.png]]

**exploit/windows/iis/iis_webdav_scstoragepathfromurl**

Por lo que vamos a explotarlo:

![[Pasted image 20250529113625.png]]

Una vez nos encontramos dentro de la maquina, vemos que hay que hacer una escalada de privilegios ya que sino no podemos ver las flags.

ejecutamos el modulo de **post/multi/recon/local_exploit_suggester**

Nos sugiere los siguientes, probamos uno a uno pero el que vale es este:

![[Pasted image 20250529113843.png]]


Hay que decir que en todos los modulos nos salia el siguiente mensaje de error:

 **Exploit failed: Rex::Post::Meterpreter::RequestError stdapi_sys_config_getsid: Operation failed: Access is denied.**

Y no nos dejaba ejecutar el exploit, tras informarme, se ve que hay que migrar a un puerto con mas privilegios.

Una vez ejecutas el exploit que te ha hecho ganar acceso  ala maquina victima, lo ejecuitas desde un proceso x , el problema es que ese proceso puede tener permisos 0 por lo que una vez dentro tendriamos que migrar a un puerto con mas permidos, eso se hace de la siguiente manera:

Una vez estamos dentro de nuestro meterpreter, tenemos que ejecutar un ``ps`` , una vez veamos los procesos de esta manera:

![[Pasted image 20250529114316.png]]

Tenemos que buscar un usuario con mas privilegios, por ejemplo el caso de **davcdata.exe** una vez tenemos uno al cual queremos migrar, tenemos que fijarnos en el **PID** y ejecutar el siguiente comando:

**migrate + [PID]**  en mi caso ``migrate 2244``:

![[Pasted image 20250529114538.png]]

De esta forma, hemos pasado a ser NT AUTHORITY\NETWORK SERVICE, por lo que ya podemos ejecutar el exploit anterior:


![[Pasted image 20250529114633.png]]


Ya lo tenemos!


![[Pasted image 20250530180115.png]]