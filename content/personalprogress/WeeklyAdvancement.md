---
title: "Weekly advancement"
date: 2020-10-19T10:18:24+02:00
draft: true
---


### Week 1
31/08/2020 - 04/09/2020

__Goal - Find out agenda__

For the first week the goal was to determine what was needed of us to accomplish for this minor and later it turned into defining the learning goals and activities that I wish to accomplish. Because this year we were required to make our portfolio with a website I had difficulties deciding on the correct framework to build the website with.

__Achieved__ 
At this point in time I was able to define my learning plan and clarified the goals of the project

__To improve__  
I wanted to already start on building the website for my portfolio. I achieved this by making a Heroku website that was React Based

### Week 2
07/09/2020 - 11/09/2020

__Goal -Hack a linux machine on Hack the Box –__

For this week my goal was to Hack a box on the suggested working environment of Hack the Box. The box that I chose was SwagShop it was rated as easy but I took it as a personal challenge to not use the write up or any of the documentation and instead try and hack it by myself without any help. This proved to take longer than I anticipated. I learned how to use several tools better because of this fact. I used Nmap and wrote several scripts to use. I used Nikto which helped me find vulnerabilities in the webpage, service port 80. I spent a lot of time trying to find more ways into the machine and lost too much time trying to inspect the website format. In the end I remembered I could use searchsploit for some of the things I discovered like the website builder framework and eventually found an exploit that looked promising. But this happened after looking at 5 others and not knowing which one to use. At this point I was able to get inside the admin page on the website where I used another exploit to get a reverse shell. This was hard because I did not know how to make a reverse shell. For the root administrator I needed help as I did not know how to escalate privileges and Google searches did not help with this matter.

__Achieved__ 

It took me the whole week to work on the machine and I believe I had some moments when I was stuck and I could have used the help of someone else or I could have consulted the write-ups without feeling I was cheating because my goal is to learn as much as possible not necessarily hack the box.

__To improve__ 

During the whole process I managed to create a note making system but I should make a habit out of making a write up for the machines that I have hacked. This will help me both to get used to making write ups and to create content for the portfolio.
 
### Week 3
14/09/2020 - 18/09/2020

__Goal - Hack the Box start another machine –__

For this week I started another box but I could not progress too far with it and realized I had connectivity issues with the Hack the Box labs. I could not connect with the VPN and I had to reinstall kali losing the notes I took on the other machine in the process. As a result, I ended up not achieving much over learning how to set up kali correctly and put a new password. I used the extra time to learn more about the OSCP exam and researched different blogs and podcasts about the process, the necessary skills and the OSCP test.

__Achieved__ 

I did not hack another box or other boxes this week but I managed to research the journey of other OSCP graduates and learned about the most important subjects in the OSCP curriculum and the one suggested by everyone to focus on was Buffer Overflow. I will continue to work on this subject.

__To improve__ 

Work more on Hack the box

### Week 4
21/09/2020 - 25/09/2020

__Goal – Create the portfolio website and report on the current status of the learning process-__

For this week the goal was to create the portfolio website. I first started by crating a React website but that proved to be more difficult than anticipated since I only knew the basics of React. I spent the rest of the week trying to learn React and concluded it will take too long to build a website from scratch for the project. I started researching other website platforms for the portfolio and I managed to find something that I could start working on that was only css, javascript and html based.

__Achieved__

Found a website model on git that I can start improving on 

__To improve__

Because I got stuck on making the website I could not write a writeup for the website and I could not update the portfolio. I should make the write-ups after I finish hacking a box or machine.
 
### Week 5 
28/09/2020 - 02/10/2020

__Goal -Read documentation for the OSCP-__

Personally, I structured my minor around the OSCP specialization. I wanted to start sooner with it but I only decided on it in the 4th week and after buying the exam it takes one week to receive the material and access to the lab. For the first week I focused on reading the materials that were supplied by them. It is a book of 800 pages and I managed to read the first 323 pages due to the technical nature of the material. It is filled with useful information but seems like I should work more on the lab.

__Achieved__

Read 323 pages through the book material of the course.

__To improve__

Start working on the video material and also finish the book documentation.

### Week 6
05/10/2020 - 09/10/2020

__Goal - Hack the box catch up + Website start –__
 

Due to finding a better platform I chose to make the website using the Hugo platform. All websites are templated and to write blogs/ posts/ entries you can use markdown. This seemed like the most useful tool and I managed to create a website that contained the learning plan for now but I will start adding the write-ups for the machines that I have hacked. Additionally, I have worked on 2 other Hack the box machines and completed them. I did this as trying the OSCP lab proved to be a harder task for now than anticipated and I will wait until I finish the reading material and go through more OSCP like Hack the box machines. All machines are documented using Cherry tree and I need to find a easy way to convert the notes from Cherry tree into write-ups written in Markdown.


__Achieved__

I finally chose a platform for my portfolio, Hugo. I converted the learning plan inside it. I Worked on 2 more Hack the Box machines and I attempted to work on the OSCP lab with no results for the moment.

__To improve__

Attempt more machines in the OSCP lab, finish reading the Documentaiton, start watching the video material for OSCP, Create write-ups for the current Machines
### Week 7
12/10/2020 - 16/10/2020

__Goal -Working on OSCP lab–__

For this week I started working from the OSCP lab material. Getting used to how to go through the lab and how to proceed. I scanned parts of the lab and tried to find interesting or potentially vulnerable machines. While I did not start any machine in particular I did learn how to go through the different machines of the lab and how it was structured. This understanding helped me in bettering myself and my skill set. 

In addition to that I wrote one Write-Up from the  HTB machines that went through.

__Achieved__
I started working in the OSCP lab and I 
__To improve__

### Week 8
26/10/2020 - 30/10/2020

__Goal - Red Team vs Blue Team –__
 

The goal of this week is to prepare and to attend the Red team vs Blue team event. During this event the red team had to attempt to hack in to the websites created by the security engineers. This proved to be a challenge as most of the applications, at least the ones that we tested were not finished or not even running, therefore there was little we could exploit. Regardless the experience was quite interesting and insightful teaching us to organize ourself as a Red Team, work as a team with other classmates and share knowledge about hte way we approach a hack. 


__Achieved__

After stuggling for the first part of the day to connect to Seclab as my credentials did not work. For the second part of the day I have scanned and found some vulnerabilites on one of the machines but sadly was not able to exploit it as we did not have the necessary time to attack it. As a result most of the day was spent enumerating, searching for exploits and trying different SQL injections and automated scripts based on the little that we found.

__To improve__

For the next R vs B event I will make sure that my Seclab credentials are working and that I know ahead of time what machines are going to be tested by my team as to be able to prepare and talk with the developers of the machines so they are up and running during the event. In addition I should polish my penetration skills.

### Week 9
02/11/2020 – 06/11/2020

__Goal -Website update–__

While work was done on the group  and on the personal portfolio assignments less time was invested into actually making the website and recording the work that was done. Because of this, updating the website so it contained my work for the midpoint hand-in was the priority of this week. Furthermore, because the main framework used to serve the website was Hugo, there were some problems with the provider to deploy the website.

At first the platform used for deploying the website was Heroku. This had a module for serving the website but it did not interpret the SASS styling correctly. As such all the images and the website looked unprofessional. Next the theme of the website was changed so that it did not contain SASS styling.

Moreover, the Heroku was interpreting the css but it did not deploy the website entirely because of the faulty and old github module that was used for this. At this point a new hosting alternative was needed. After some inspection a free and better alternative was selected, Netlify. This was suggested for serving Hugo based websites and it had a free hosting and deployment service. 

__Achieved__
Even though I did not work on Buffer Overflow as I originally planned I learned a lot about deploying a website and about the different options that are available for this. I discovered Netlify and I furthered my understanding of the Hugo framework.

__To improve__
Before choosing a development stack it would be recommended to inspect the recommended infrastructure to deploy it on as a live environment is different from a controlled local environment deployment.

### Week 10
09/11/2020 – 13/11/2020

__Goal - Stack overflow HTB machines + Updating the website–__

As in the previous week there were difficulties on working on  the buffer overflow  because of delays caused with the  portfolio website deployment, working with the buffer overflow proved to be more difficult. This week I practiced one more overflow machine Enterprise and wrote write-ups for the machines in order to receive more feedback on my portfolio from my Red Teaming teacher.


__Achieved__
During this week the portfolio website has been successfully updated with write-ups and based on them feedback has been received for what needs to be changed in the website.

__To improve__
Start working on OSCP material more and have a better format to transform the notes you take on machines for writeups.

### Week 11
16/11/2020 - 20/11/2020
__Goal -PWK book exercises–__

During this week I worked on the exercises from the book. 
__Achieved__
Worked on the first 6/25 chapters exercises
__To improve__
Work on machines inside the lab

### Week 12
23/11/2020 - 27/11/2020

__Goal -PWK book exercises + OSCP lab–__

I continued work on the exercises from the book 11.2.5(Buffer overflow on windows)/25 and I also finished a machine inside the lab.

__Achieved__:
Achieved  user shell on one machine inside the lab

__To improve__

Gain root access to the machine and work on making OSCP style reports

### Week 13
30/11/2020 - 04/12/2020

__Goal - Lockpicking practice + Student presentation at TQ5–__

Got the lock picking kit from the ISSD and started learning how to use it. Also the student presentation in TQ5 did not take place, no explenation was given to why but the extra time was used to keep working on the OSCP lab exercises.

__Achieved__

Got the lock picking set. Started learning how to use it.

__To improve__

Put more work into OSCP lab machines.

### Week 14
07/12/2020 - 11/12/2020

__Goal - Preparation for the event + OSCP lab machines + Lockpicking practice–__

Being the last week the lock picking set could be used one day was spent preparing all the locks and filming myself for proof. For next weeks event added a virtual machine to use during the event, made sure connecting to it is possible. In addition a list of commands for the event where prepared and additional resources where noted.

Continued work on the machine from 2 weeks without getting root shell, decided to work on another machine.

__Achieved__: Finished unlocking all locks from the lock picking set. Made preparations for the final red vs blue team event. continued work on the PWK lab book exercises

__To improve__
Make sure that I do both tcp and udp scan on the machines. Practice brute forcing with hydra.

### Week 15
__Goal - Red Team vs Blue Team Event 2 –__

Spent one day working for the Red Team vs Blue team event, worked together with group 5 to attack a different set of machines than last time.

In addition to this worked together with the project team on finishing

__Achieved__: Found a number of vulnerabilities inside the system that was tested. Worked on our team red teaming skills

__To improve__

### Week 16
__Goal - Red Team vs Blue Team –__

__Achieved__

__To improve__