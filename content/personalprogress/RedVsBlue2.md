---
title: "Red vs Blue | Event 2"
date: 2020-12-14T17:06:01+01:00
draft: false
summary: "The description of the second Red vs Blue event
"
---

## Event 2
The  Red vs Blue event took place on Monday 14/12/2020. Similar to the previous event the goal of the red team was to attack the applications created by the green team students. In the meanwhile the blue team was tasked with monitoring the activities of the red team and identifying their activities inside the network.

Compared to the last event preparations were made ahead of time to make sure that connecting to Netlab is not an issue and that all tools and commands that might be used are already bookmarked, shared with the group and available for use. Before the event all the Red team groups agreed on the applications they would work on. Because the teams in which we functioned remained the same this event was more organized and we were able to have all our applications to test and the task distribution done in first 30 minutes of the event.

<!-- TODO: Make sure to add the links that you saved for this event and mention that you also looked up a better report format to work on. -->

The event was split into 3 parts. The first one in the morning from 9:00 until 11:45 where we were allowed to attack the machines. Compared to the first event everything was set in place so after dividing the workload between all group members we started enumerating on them. At the end of the first part we were able to enumerate on all machines, some of the other teammates had problems deploying their machines so the team decided to work together on the same application.

In the second part of the event from 13:00 to 15:00 the team already found most of the possible vulnerabilities of the websites and we each tried to attack and exploit parts of them. 

Finally in the third part of the event we presented our findings. Even though we were able to find some possible vulnerabilities we were not able to exploit them fully. Some attempts were made.

### Applications to test



![Work Distribution](/RedvsBlue2//DistributionOfWork.png)
#### [Figure 1.1 - Distribution on work between teams] 
For this event we decided in between all  Red team groups to switch our 
applications between groups as such our team took the applications that were previously attempted by group 4.


```
An online banking system:
Test the data encryption and the input filtering, jwt authorization, captcha, Password rules,

user account:      firstname:  Red  lastname:  Team password: H@ckTh1s
admin account:     firstname:  Red  lastname: Admin password: N3wPages!

10.10.1.110:4200
1472_Client1-VLAN
--------------------------------------------------------------------------
"See in teams Documentation -> Test object clarification Bart Janssen  <-
Test for: Data encryption, authentication, token steal/intercept, MitM, 2FA, digital licensing, retrieving data from other users"

See in 'teems red blue chanel -> Documentation -> Test object clarification Bart Janssen'

"Server: 10.10.2.125 
Client 1: 10.10.2.126 
Client 2: 10.10.2.127 "    
1473_Client2-VLAN
 
--------------------------------------------------------------------------
HTES AAS Factory4.0 project:  

See how far you can get ;). First try to hack some of our systems, on 145.220.75.127 (accessible without any VPN (but it is (strongly) recommended to use your own VPN cause of Seclab’s outside firewall). If you give up, you could ask me privately questions (after 13.00 UTC+1), so that you can test our inside network devices behind the security.

Ask me private pls.    

145.220.75.127

--------------------------------------------------------------------------
Webportal for managing climate systems: 
Test for :input validation, authentication, authorization, recaptcha at regestration    

Accounts can be registered in application, admin accounts are not implemented    

10.10.2.140    
1473_Client2-VLAN
```

### "a online Banking System Applicaiton" application

For this team 5 the team I was part of functioned  with the same members. In total we had 4 members so we divided each application to each of us. 

During this event I worked on the first application on the list which I believe was wrongly named "a online banking system application" (with a insted of an).

For most of the first part of the test I worked on enumerating the ports and checking the passwords strength, the functionality of the website and looking for outdated software.

![Trying the website](/RedvsBlue2//LoginPage.png)
#### [Figure 2.0 - Failed Login] 

As the application creator wanted we first tried to login into the website with the credentials that were supplied by him. In the begining we believed that the login credentials were wrong so we messaged the creator. He let us know that they are correct so we continued our test. In the later part of test the login functionality worked so it might have been that we did not input the credentials correctly.

Because loging in was not yet an option it was decided that the machine/ ip that was given should first be scanned. For this a tcp nmap scan was first done and then and UDP scan

![Trying the website](/RedvsBlue2//TCPScan.png)
##### [Figure 3.0 - ScanFindings]

After scanning the machine in addition to the given port 4200 which is commonly used for Angular websites another 2 useful ports were found port 5001 and ort 5341. One lead us to the Swagger API page which can be used to call api commands on the website. And another on port 5341 for Seq which is a monitoring solution.

![Trying the website](/RedvsBlue2//SwaggerAPI.png)
##### [Figure 4.1 - Port 5001- Swagger API]
Swagger is an open APi development tool for users, teams and enterprises. The fact that this is accesible to normal viewers that do not have permissions is a vulnerability in itself. After looking at the other suspicious port we tested the API functionality to see if any user can use it.

![Trying the website](/RedvsBlue2//SEQ.png)
##### [Figure 4.2 - Port 5341 - Seq]
As this was another login page that we were not intended to find we believe that this port should not be open for everyone to see. The fact that we could see this port and that we could access this part of the website was a problem in itself. Luckily, it was not easily accessible. We tried the most common login passwords for webpages like admin-admin, admin-password, root-password etc.

![Trying the website](/RedvsBlue2//SwaggerAPIendpoint.png)
##### [Figure 5.0 - Swagger API testing the API endpoints]
From the begining we speculated that this might be a major vulnerability and we were able to confirm that we could indeed call the api endpoint without getting any errors. One of the endpoints that seemed to create an account for this website /Authentication/create. 

By calling this endpoint using curl we were theoretically able to create an account to login. The problem was that the created user did not work and as such we assumed it was protected but later on we realised that that functionality was simply not fully implemented yet.

As such we could still call this the biggest vulnerability of the website.

![Trying the website](/RedvsBlue2//userLogin.png)
##### [Figure 5.1 - Testing the actual functionality]

Because we wanted to test our newly created user we started trying the login into the user account/ normal page. We were not able to login with the newly created credentials but we were able to login with the given credentials.
```user account:      firstname:  Red  lastname:  Team password: H@ckTh1s```


![Trying the website](/RedvsBlue2//userLoginInscpect.png)
##### [Figure 5.2 - Looking into the website]

There was not much that we could test do with the website itself, the captcha was not working so we could not test it we inspected the cookies for the webpage to look for interesting information but we did not find anything useful. As such the next step taken was to look into the admin webpage.

![Trying the website](/RedvsBlue2//adminLogin.png)
##### [Figure 6.0 - Admin page]

This page was in english and as such it was much easier to understand what it was doing. Sadly from the looks of it the website was not finished as the create user functionality did not work. That and it allowed us to make as many users as we wanted with the same names and passwords. It did prompt for passwords containing letters and signs so that was a good sign.

In addition something that we noticed was missing from the website was the logout page. To test an incognito page was used to erase the cookies and use a different page. Something that we suggested at the end of the presentation was a logout button.

![Trying the website](/RedvsBlue2//BruteforceSEQ.png)
##### [Figure 7.0 - SEQ hydraBruteforce]
In spite of the website being hard to test and to understand we continued from where we left off with the SEQ dashboard. We tried bruteforcing it with hydra which did not yield and useful results.


![Trying the website](/RedvsBlue2//buirpbruteforce.png)
##### [Figure 8.0 - SEQ BuirsuiteBruteforce]
Next the team tried to do a repeater injection with Buirpsuite in the hopes that hydra was the problem. Due to a constraint of time the team was not able to finish the attack. Nonetheless, with more time the team believes that we could have gotten inside this part of the website and that is a potential vulnerability that is easy to fix and prevent.






### Group report and presentation

For the last stage of the event the teams were required to present their findings. To this end we created a document with our findings and a group report. As most of the findings we had were the accomplishment of the group we decided to state our work as such. These findings can be found in the 
<!-- TODO: Still need to make the redteam document and include it in here -->
[group document](https://docs.google.com/document/d/1rLfha397ISHSdsXsOmb3bVRcS5ejauKUF-q4mHzbsIg/edit#) and the [group presentation](https://docs.google.com/presentation/d/1e0r3fgg7E5uUCzh1KgLpszqBzHL6_j9aalGyg7PmM0k/edit#slide=id.ga50b1d7492_6_2)





