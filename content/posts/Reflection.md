---
title: "Personal reflection"
date: 2020-12-14T17:06:01+01:00
draft: false
summary: "Personal reflection on the goals of the project and the goals of the team project"
---

#### Personal Development
------------
##### (Goals)
The goals of this semester for me personally was to better my skills in penetration testing so as to become more confident in my ability to understand and perform tasks in this field. To achieve this goal I set out to follow the PWK course that leads to the OSCP certification.

Throughout the minor, I have had ample opportunity to work on this course and to seek advice and help from my fellow students and mentors. This has aided me in making better decisions in regards to planning, tooling used, and reporting.

##### (Achievements)
Overall I have complete 53 machines inside the PWK lab environment, went through the lab material which meant doing exercises related to all the subjects for the OSCP exam. This meant scanning, enumerating passively, enumerating in a compromised machine, exploiting different types of vulnerabilities (web-based, network-based, application-based, information-based), and then being able to form footholds and gain higher privileges on machines. On top of that, I participated in the Red vs Blue team events and also found time for my own part-time job as a junior developer/infrastructure engineer in my company.
##### (Challenges)
However, there were a number of challenges that I needed to overcome. The main one being the fear of working on live machines because most machines do not come with a predefined exploitation manual one must find how to exploit them themselves. Many times I was discouraged to continue because I was not able to find anything inside the scans or that the machines that I was working on were broken/unreverted.

This has led me to learn why the motto of the course is Try Harder. Because the only way to finish is to keep trying different things until you find something. I found that once I accepted that it will not work from the first time and took everything slow and step by step I would be able to find the mistakes that I made much easier and analyze the scans better.

A second challenge that I faced was time management. Between the OSCP lab manual, the machines that I needed to work on, the group project for the minor, and my personal part-time job little time remained to organize all the work that I did. If I were able to redo this semester I would try to manage my time outside of the school program better so that I would have had time for the penetration test as well, for working more on the lab machines and making more detailed reports so that I could practice my procedural skills more. That being said my solution of working at the weekends has helped me built a better work ethic.

##### (Setbacks)
Furthermore, I had some setbacks as well throughout my personal development process. The main setback that I faced was the portfolio that needed to be made through a website. Based on all my previous knowledge I assumed that it would be easy to create a website in a new framework that I have never worked with before and keep up with all the other tasks. Sadly I was mistaken. In the beginning, I had hoped to create my portfolio with the use of React, a platform new to me. Later on, I discovered that it was too difficult so I looked for prebuilt templates for websites. This also proved to be harder to manage as more time was put into creating the website and little changes than actually on the lab. Therefore the final solution was to find a platform that would do all the front end for me leaving a minimum amount of creating to me. Such a platform was Hugo (an alternative considered as WordPress).

On this platform, I was able to build my website but deploying it was much harder. I had to go through multiple hosting platforms Heroku, Fontys Hera Servers, Netlify. In the end, the one that worked was netlify. The problem with hosting it was that Hugo requires certain commands to be run on the hosting machine that the Hera server could not run (because it did not have Hugo preinstalled). And the Heroku servers did not have good frontend modules for Hugo. Therefore the final solution was Netlify.

From this, I learned that the next time I work on a web-based portfolio I should focus on choosing technology and a platform that I already have experience with because there will be enough work to be done with the process of creating and filling the portfolio.



#### Team project
----------------
##### (Goals)

At the start of the project, the team set out to work together with the Fontys network administrators to aid them in some of the team's selected areas. What we chose to investigate was how to create a dashboard for displaying useful information and creating scripts for managing the Netlab environment.

As such in the first weeks of the project, we worked on a plan to divide the problems into a plan that could be accomplished in the time frame that was given to us, 17 weeks.
##### (Achievements)
In these 17 weeks, we managed to accomplish what we set out to do. That is research and implement ways to display the information that the stakeholders of Netlab wish to have displayed in a dashboard and enforce the Netlab environment policies through the use of scripts.

Because working directly on the Netlab environment was not possible we were forced to create our own Vcenter environment. Thankfully with the help of Fontys and the knowledge of our teammates we were able to create a similar environment to Netlab where we developed all our deliverables.

On top of that, we researched data visualization technologies like the ELK stack and Grafana, we made surveies filled up by students on data that they want to see and we had ample communication with the network administrators about the requirements .Furthermore, we created a website dashboard(that sadly we did not have enough time to finish and implement in their system).

##### (Challenges)
That being said we had a number of challenges, the first one was the working environment. Because in the first Sprint we did not yet have a test environment provided by the network administrators most of it was spent on creating applications that were untested. What is more, in the end we were provided with a physical server that we needed to transport ourselves to our homes and then to host ourselves. Managing the payments for hosting it, making it available on the internet, paying for electricity were all managed by the team. To solve this problem we had to outsource this problem through our own contacts.

Luckily, we were able to do this in time for the first sprint. The next challenge we faced was learning all the new technologies for the platforms we were using. We had to learn how to write scripts in PowerShell, program in angular, and manage Vsphere with the use of Powershell scripts. Throughout the second sprint, we were able to overcome these challenges as well.

In the final sprint, we had to decide to give up on the website dashboard because of the realization that all the setbacks put us in a situation where we could no longer develop a working and secure website. What we ended up doing was implementing all the remaining functionality in the Grafana dashboard where we created all the graphs and where we could also implement user authentication and alerting.

##### (Setbacks)

The biggest contributor to this project's problems was the number of setbacks it had. Each sprint came with a number of them.

In the first sprint, we had difficulties coming in contact with a large number of stakeholders and after that, the Product Owner had to change so the time that took was a time that was lost for the project.

Afterward, in the second sprint 2 of our colleagues decided to leave the project leaving more work for fewer people. On top of that, the Netlab environment was down for most of the end and beginning of the sprint therefore we could not test our scripts and deliverables in the live environment, and contacting the network administrators became harder.

Finally, in the third sprint, we were not able to test our deliverables at the beginning of the sprint and then had problems with the insufficient permissions for our machines inside the environment to collect the data that was needed to create the graphs.

In the end, we were able to manage because the team had good communication and some pre-knowledge of working with the Vsphere environment but in the future, I would make sure that both my team and the stakeholders take better notice of the deadlines set at the start of the project and build the project around resources that are already available not those that need to be made available.


#### Conclusion
-----------

I consider that over this minor I have grown a lot as a cybersecurity specialist, team leader, and software developer. All the new things I have learned, the challenges that I have faced, and the setbacks that I overcame helped me build a better work ethic, manage my time better, and communicate with teammates and stakeholders much better.

In my opinion, this semester has been a success and I would recommend it to other students as well, having to build your own learning plan and being held accountable for it by yourself in the first place helps one develop not only as a professional but also as a person. At the end that is what is most important.












