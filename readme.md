****

# CTF Write-Up Pickle Rick (TryHackMe)



**Realizado por: Raúl Cabrera Garcia** 

***Descripción: Se trata de la guía-paso, para poder resolver está máquina CTF***


***Instrucciones:*** 

- Aunque en la misma descripción de la máquina, nos explica que podemos explorar la aplicación web en:  https://IP_MAQUINA.p.thmlabs.com
- Primero, realizamos un escaneo con NMAP de los puertos disponibles:
![FOTO1](/fotos_write_up/foto1.png)
- Segundo, vemos que tenemos 2 puertos el 22 (SSH) y el 80 (HTTP o Web). En este caso lo primero sería dirigirnos a la web.
- Tercero, A modo de explicación, nos comenta que "Rick necesita nuestra ayuda, la ayuda de Morty. Para ello, debemos entrar en su ordenador y encontrar los 3 ingredientes secretos para terminar su poción. Pero no recuerda la contraseña".
FOTO 2
- Cuarto, lo primero que podemos mirar es el "robots.txt", para ver si se puede acceder algún directorio. Que no indexa el buscador.
Vemos una palabra "Wubbalubbadubdub", pero si probaís, no os lleva a ningún directorio. Paralelamente, podemos ir realizando un "gobuster", contra la web. Para así,
poder listar posibles directorios u archivos, disponibles.
```
gobuster dir -u http://IP_MAQUINA -w /usr/share/dirb/wordlists/common.txt -x php
```
- Quinto, si miramos el código fuente de la página principal de la web, a modo de comentario, encontraremos una primera pista. Una nota así mismo, a Rick.
Con el "Username: R1ckRul3s"
FOTO 3
- Al terminar gobuster, veremos que nos encontro varios archivos .php. Y otros que son enlaces a un "login.php". 
FOTO 4
- Lo único que tenemos aquí, es el usurio, encontrado como comentario en el código fuente y aquella palabra rara; encontrad en el "robots.txt". Lo probamos como
usuario y clave en este login. Y entramos a un panel donde podemos ejecutar comandos de terminal.
```
usuario: R1ckRul3s
clave: Wubbalubbadubdub
```
FOTO 5
- Hacemos un "ls" para ver que tenemos en el actual directorio que estamos. Y encontraremos el archivo "Sup3rS3cretPickl3Ingred.txt", es el que querremos
ver en principio.
FOTO 6
- Si intentamos hacerle un "cat", no saldrá un mensaje de que ese comndo está deshabilitado junto al "gif". Por lo que podemos hacer es acceder a el directamente
por la url de la web. Ya que sabemos que estamos en "/var/www/html".
```
http://IP_MAQUINA/Sup3rS3cretPickl3Ingred.txt
```
Obtenemos el primer ingrediente.
FOTO 7
- Si volvemos a mirar la lista de ficheros, al hacer el "ls", vemos otros fichero de texto llamado "clue.txt", si accedemos igual que el anterior. Nos dice
FOTO 8
- Debemos ver el sistema de ficheros, si vemos que podemos acceder a listar cualquier lugar del sistema, lo principal es al "home" de posibles usuarios.
Listamos y vemos que tenemos dos usuarios con home "rick" y "ubuntu". En este caso nos interesa el de "rick", para conseguir el seguno ingrediente.
Hay un archivo dentro del directorio rick que se llama "second ingredients". Para poder verlo como hemos visto antes no podemos usar cat. Y ahora, tampoco
podemos acceder desde la url. Por lo que usaremos un comando que nos permita ver un "resumen", de lo que tiene. Usamos el comando "less".
```
less '/home/rick/second ingredients'
```
Veremos de esa manera el segundo ingrediente.
FOTO 9
- Ahora debemos ir a por el último ingrediente. Al tratarse en este caso de ir buscando "ingredientes", el último para encontrarlo. En otro tipo de máquina
nos tocaría el ir a buscar una flag en "/root", pero para ello primero debemos tener permisos suficientes.
- Primero si pruebas, hacer un "ls /root", vemos que no lo tenemos disponible. Si probamos hacer un "sudo -l", para ver que podemos ejecutar como "sudo".
Vemos que el usuario "www-data", que es el que somos. Puede ejecutar cualquier comando como sudo. Sin necesidad de clave.
FOTO 10
- Por lo que si ejecutas:
```
sudo ls /root
```
- ¡¡SORPRESA!!, podemos ver el contenido y vemos el archivo "3rd.txt", que es el tercer ingrediente. Como hemos hecho antes para leerlo ejecutamos un "less".
FOTO 11
- Aquí vemos el nombre del último ingrediente. Enhorabuena, Morty!!. Rick, podrá realizar por fin su poción!!
FOTO 12


***Flags:***

Primera Flag: mr. meeseek hair
Segunda Flag: 1 jerry tear
Tercera Flag: fleeb juice
