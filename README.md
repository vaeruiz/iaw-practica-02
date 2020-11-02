# Práctica IAW 2

# Implantación de una aplicación web LAMP en AWS

## Creando la máquina en AWS

Iniciamos sesión en AWSEducate y entramos en la pestaña "Mis clases". Entramos en la clase de IAW y se nos abrirá una pestaña Workbench que nos mostrará el estado de nuestra cuenta AWS, debajo de esta información veremos dos botones, uno que nos sirve para obtener información más detallada de nuestra cuenta, y otro para acceder a la consola de AWS, seleccionamos el botón de la consola para acceder a los servicios de Amazon Web.

Se nos abrirá una nueva pestaña en la que veremos todos lo servicios disponibles, entramos en el servicio

>Ejecute una máquina virtual con EC2

Entraremos directamente a la creación de la máquina virtual. A primera vista tendremos todos los sistemas operativos de los que disponemos para hacer nuestra máquina, buscaremos el sistema operativo Ubuntu Server 20.04 LTS y le daremos a Select.

Después elegimos el tipo de instancia que queremos, solo tenemos un tipo de instancia gratuita así que si no está marcada, elegimos la instancia t2 micro y le damos a Next.

La configuración de los detalles de la instancia lo podemos dejar por defecto, así que pasamos a la siguiente opción de almacenamiento.

Podemos dejar el almacenamiento que viene predeterminado, que son 8GiB, si queremos más espacio escribimos la cantidad de GiB que queremos que tenga el disco duro, en mi caso lo dejaré con la cantidad que viene por defecto. Después le damos a Next.

Podemos añadir alguna etiqueta a la máquina para poder diferenciarla de otras máquinas que tengamos, ahora podemos omitir este paso porque solo vamos a tener una máquina, pero está bien que tengamos esta opción en cuenta por si creamos más máquinas en el futuro. Las etiquetas se pueden añadir en cualquier momento.

Pasamos a la configuración de seguridad, aquí debemos añadir dos puertos a nuestra máquina, uno será un puerto tipo HTTP, y el otro HTTPS, le damos añadir regla y en el tipo buscamos HTTP, cuando lo localicemos lo seleccionamos y automáticamente escogerá el puerto de acceso que en este caso será el 80, hacemos lo mismo para el puerto HTTPS que su puerto de acceso será el 443. Cuando hayamos hecho esto le damos a Ver el resumen de la máquina y lanzar.

Si todas las opciones están correctas y estamos conformes con nuestra máquina pasaremos a crear nuestro archivo de claves, al darle al botón de lanzar la máquina se abrirá una nueva pestaña en la que podremos elegir si crear las claves de acceso o elegir un archivo ya existente, yo crearé un par de claves nuevas bajo el nombre de asirpacrAWS, este archivo lo podemos meter en un pendrive por si queremos conectarnos a la máquina desde otro equipo.

Una vez hayamos creado el archivo lo descargamos y lanzamos la máquina.

Nos vamos al panel de instancias y veremos el ID de nuestra instancia y al lado el estado de esta (detenida, en ejecución, pendiente).

Ahora veremos como conectarnos a ella.

## Conectándonos a la máquina

Para conectarnos a la máquina necesitamos saber la dirección DNS de esta, esto se puede saber entrando en nustra máquina desde el panel de instancias, en el resumen de la máquina, veremos un apartado llamado DNS de IPv4 pública.

El cliente que usaremos para conectarnos por SSH será Visual Studio Code con el plugin SSH. Con el plugin instalado, tendremos que configurar el archivo de host para realizar la conexión.

Pulsamos la combinación de teclas Shift+CTRL+p y escribimos ssh, se nos desplegarán unas opciones, buscamos la que dice Remote-SSH:Connect to Host y pulsamos enter, a continuación entramos en la opción que dice Configure SSH Hosts y seleccionamos el archivo de configuración que está definido de la siguiente manera

>C:\Users\TuUsuario\.ssh\config

Entramos en ese archivo y tendremos que poner la siguiente información

```config
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host Nombre de host, podemos ponerle el que queramos
    HostName Aquí pondremos nuestra dirección DNS pública
    User El usuario de la máquina (por lo general suele ser ubuntu)
    IdentityFile Donde está ubicado el archivo de claves, poner una ruta absoluta
```

Podemos añadir este conjunto en nuevos párrafos para poder conectarnos a más de una máquina de forma directa y no cambiar lso datos del fichero, es decir, en vez de tener el fichero como se ha mostrado, lo podemos tener de la siguiente forma:

```config
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host Máquina 1
    HostName nombreDNS1
    User ubuntu
    IdentityFile c:\users\fichero.pem

Host Máquina 2
    HostName nombreDNS2
    User ubuntu
    IdentityFile c:\users\fichero2.pem
```

Cuando hayamos configurado el archivo lo guardamos y volvemos a desplegar los comandos de VS, pero ahora al entrar en la opción Connect To Host encontraremos el nombre de la máquina que hemos puesto (iaw-práctica-2), si los permisos del fichero están bien (tienen que estar en 400) Visual Studio nos conectará a la máquina y se abrirá una nueva ventana desde la que podremos abrir una terminal y utilizar la máquina.

## Instalando pila LAMP

Vamos a pasar el script que creamos en la práctica anterior y lo vamos a ejecutar para instalar la pila LAMP en nuestra máquina.

Creamos un documento dándole a File>New File, y cuando nos pregunte si queremos crear el archivo en la máquina local o en la máquina AWS seleccionamos la máquina AWS. Con el archivo abierto, podemos abrir a la par nuestro script y copiar el contenido que hay en él para pegarlo en el archivo de la máquina AWS. Lo guardamos y cambiamos añadimos los permisos de ejecución al script con el comando

>sudo chmod +r script.sh

Después ejecutamos el script con el comando

>sudo./script.sh

Y dejamos a nuestra máquina instalando la pila LAMP y las aplicaciones extras.

Para comprobar que ha funcionado podemos acceder a la máquina a través del navegador web poniendo su dirección DNS en la barra de búsqueda