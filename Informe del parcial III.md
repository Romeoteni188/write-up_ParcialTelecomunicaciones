***

## Fase 1 Reconocimiento de las ips nivel general del pool 192.168.1.0/24

![[Pasted image 20241109183812.png]]


## Fase 2 Enumeracion de Servicios 

Como objetivo principal antes de enuemerar los servicios fue la deteccion del IDS wazuh
los servicios y los puertos que usa wazuh son:
ssh 22
111 tcp
443 tcp
![[Pasted image 20241109184841.png]]


aparte wazuh tiene estos tres puertos, los agentes solo cuentan con solo uno puerto 

![[Pasted image 20241109184744.png]]

![[Pasted image 20241109184921.png]]

## Fase POC 
El servidor wazuh era la ip 192.168.1.111

otra desventaja que tuvieron el otro equipo que no conocian el IDS  como tal existia de un segundo usuario.
![[Pasted image 20241109185342.png]]

Esta claro en la documentacion el usario kiabanaserver y wazuh-user> 
https://documentation.wazuh.com/current/search.html?q=kibanaserver&check_keywords=yes&area=default

Es por eso que no era necesario hacer un ataque de fuerza bruta

![[WhatsApp Image 2024-11-08 at 6.53.15 PM.jpeg]]
Con esta informacion ya no era necesario las demas ips que tambian habian maquinas virtuales.

El objetivo se estaba siendo bien claro que la maquina que estaban protegiendo era la linux ubuntu con la version 16

Se Escaneo de puertos al victima ubuntu y tenia los puertos 
80,22,443, 21 

Lo primero  que se hizo es ver que tenia ese puerto 80 en el navegador
![[Pasted image 20241109192736.png]]
Despues toco enumeros recursos compartidos 

![[Pasted image 20241109192821.png]]

![[Pasted image 20241109192838.png]]
se uso otra herrienta para ver mas de los recursos 
![[Pasted image 20241109192908.png]]
vemos que existe la ruta secret
![[Pasted image 20241109193022.png]]

![[Pasted image 20241109193038.png]]

Examinado la url el camino no era alli para la intrucion

El puerto 21 tenia una version deprecada que tenia una version de 1.3.3, toco investigar sobre ese servicio.


![[Pasted image 20241109193405.png]]

Descripcion de la vulneravilidad 

El domingo 28 de noviembre de 2010, alrededor de las 20:00 UTC, el servidor de distribución principal del proyecto ProFTPD se vio comprometido. Los atacantes probablemente utilizaron un problema de seguridad no corregido en el demonio FTP para obtener acceso al servidor y utilizaron sus privilegios para reemplazar los archivos fuente de ProFTPD 1.3.3c con una versión que contenía una puerta trasera. La modificación no autorizada del código fuente fue detectada por Daniel Austin y transmitida al proyecto ProFTPD por Jeroen Geilman el miércoles 1 de diciembre, y corregida poco después.

Cualquiera que haya descargado ProFTPD 1.3.3c desde uno de los servidores de réplica oficiales entre el 28 de noviembre de 2010 y el 2 de diciembre de 2010 probablemente se verá afectado por el problema. La puerta trasera introducida por los atacantes permite a los usuarios no autenticados acceder remotamente como root a los sistemas que ejecutan la versión modificada maliciosamente del demonio ProFTPD.

```bash
https://github.com/shafdo/ProFTPD-1.3.3c-Backdoor_Command_Execution_Automated_Script.git
```

```bash
https://www.aldeid.com/wiki/Exploits/proftpd-1.3.3c-backdoor
```

## Fase de LPE

La escalada de privilegios locales, cuando ya tienen acceso al sistema y estan intentando elevar permisos dentro del mismo.

para explotar la vulnerabilidad se uso  searchsploit
![[Pasted image 20241109193623.png]]
![[Pasted image 20241109193701.png]]

Con ese exploit uno ya podia ingresar como Root al servidor 
se enumero las posibles contraseñas

![[Pasted image 20241109193810.png]]
en las ruta podemos listar las posibles contrañas y usuarios que se guardaron en dos archivos uno para los usuarios y el otro para las contraseñas , tambien se lista los grupos
cat /etc/passwd

![[Pasted image 20241109194107.png]]
para finalizar se escaneo los servidores de la ip  192.168.1.104 y 192.168.1.101
la 1.101 tenia estos servicios y puertos abiertos.

![[Pasted image 20241109194730.png]]
![[Pasted image 20241109194803.png]]
La otra ip 1.104

![[Pasted image 20241109194948.png]]

![[Pasted image 20241109195014.png]]


# Continuará . . . !
