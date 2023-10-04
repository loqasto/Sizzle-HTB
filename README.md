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

