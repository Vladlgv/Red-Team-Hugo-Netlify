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
