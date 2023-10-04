# Sizzle-HTB
Máquina Sizzle de HackTheBox

Escaneo inicial con nmap

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
          49664/tcp open  unknown
          49665/tcp open  unknown
          49668/tcp open  unknown
          49669/tcp open  unknown
          49676/tcp open  unknown
          49688/tcp open  unknown
          49689/tcp open  unknown
          49691/tcp open  unknown
          49694/tcp open  unknown
          49699/tcp open  unknown
          49708/tcp open  unknown
          49714/tcp open  unknown
          
          Nmap done: 1 IP address (1 host up) scanned in 39.66 seconds
