## Fase 1 Reconocimiento de las IPs en el pool 192.168.1.0/24

![Reconocimiento de IPs](images/Pasted_image_20241109183812.png)

## Fase 2 Enumeración de Servicios

Como objetivo principal antes de enumerar los servicios fue la detección del IDS Wazuh.  
Los servicios y puertos que usa Wazuh son:  
- ssh 22
- 111 tcp
- 443 tcp

![Servicios de Wazuh](images/Pasted_image_20241109184841.png)

Además, Wazuh tiene estos tres puertos; los agentes solo cuentan con un puerto.

![Puertos adicionales](images/Pasted_image_20241109184744.png)
![Más puertos](images/Pasted_image_20241109184921.png)

## Fase POC

El servidor Wazuh tenía la IP 192.168.1.111.

Otra desventaja que tuvo el otro equipo fue que no conocían el IDS, y existía un segundo usuario.

![Usuario adicional](images/Pasted_image_20241109185342.png)

Está claro en la documentación el usuario `kibanaserver` y `wazuh-user`:
[Documentación de Wazuh](https://documentation.wazuh.com/current/search.html?q=kibanaserver&check_keywords=yes&area=default)

Es por eso que no era necesario hacer un ataque de fuerza bruta.

![Captura de información de usuario](images/WhatsApp_Image_2024-11-08_at_6.53.15_PM.jpeg)

Con esta información, ya no eran necesarias las demás IPs, aunque había máquinas virtuales adicionales.

El objetivo era claro: la máquina protegida era una Linux Ubuntu versión 16.

Se escanearon los puertos de la víctima Ubuntu, que tenía los puertos 80, 22, 443 y 21 abiertos.

Primero, se revisó el puerto 80 en el navegador.

![Revisión del puerto 80](images/Pasted_image_20241109192736.png)

Luego, se enumeraron recursos compartidos.

![Recursos compartidos](images/Pasted_image_20241109192821.png)
![Más recursos compartidos](images/Pasted_image_20241109192838.png)

Se usó otra herramienta para ver más de los recursos.

![Herramienta adicional](images/Pasted_image_20241109192908.png)

Se encontró la ruta `secret`.

![Ruta secreta](images/Pasted_image_20241109193022.png)
![Ruta secreta - 2](images/Pasted_image_20241109193038.png)

Examinando la URL, el camino no estaba allí para la intrusión.

El puerto 21 tenía una versión deprecada 1.3.3, se investigó sobre ese servicio.

![Puerto 21 versión deprecada](images/Pasted_image_20241109193405.png)

### Descripción de la Vulnerabilidad

El domingo 28 de noviembre de 2010, alrededor de las 20:00 UTC, el servidor de distribución principal del proyecto ProFTPD fue comprometido. Los atacantes utilizaron una vulnerabilidad no corregida en el demonio FTP para obtener acceso al servidor y reemplazaron los archivos fuente de ProFTPD 1.3.3c con una versión que contenía una puerta trasera.

- [Script de ejecución de comando automatizado](https://github.com/shafdo/ProFTPD-1.3.3c-Backdoor_Command_Execution_Automated_Script.git)
- [Detalles de la vulnerabilidad](https://www.aldeid.com/wiki/Exploits/proftpd-1.3.3c-backdoor)

## Fase de Escalada de Privilegios (LPE)

La escalada de privilegios locales se realiza cuando ya se tiene acceso al sistema y se intenta elevar permisos dentro del mismo.

Para explotar la vulnerabilidad, se utilizó `searchsploit`.

![Searchsploit](images/Pasted_image_20241109193623.png)
![Exploit encontrado](images/Pasted_image_20241109193701.png)

Con este exploit, se logró acceder como root al servidor y se enumeraron posibles contraseñas.

![Contraseñas](images/Pasted_image_20241109193810.png)

En la ruta se listaron las posibles contraseñas y usuarios, almacenados en dos archivos: uno para los usuarios y otro para las contraseñas, además de los grupos:

cat /etc/passwd

![contraseñas](images/Pasted_image20241109194107.png)


para finalizar se escaneo los servidores de la ip  192.168.1.104 y 192.168.1.101
la 1.101 tenia estos servicios y puertos abiertos.


![contraseñas](images/Pasted_image20241109194730.png)

La otra ip 1.104

![contraseñas](images/Pasted_image20241109194948.png)

![[Pasted image 20241109195014.png]]

![contraseñas](images/Pasted_image20241109195014.png)

# Continuará . . . !
