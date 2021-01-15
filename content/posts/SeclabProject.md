---
title: "Seclab-Project (Technical Proof)"
date: 2020-12-08T13:10:54+01:00
draft: false
summary: "
An overview of the project conducted together with a team of 6 other students with the goal of researching and developing ways to improve the Netlab environment monitoring system."
---

### Project Description
 Fontys ICT is interested in an improved version of their SEALAB virtual environment that provides a monitoring solution, visualizing, and interacting with the SECLAB environment in a more secure and user-friendly way.

 Now Fontys ICT is experiencing difficulties with visualizing the global state of the environment and has a delay in response to problems as they cannot find the proper remediation for it in time. Based on this and the demand from teachers, students, and administrators the need for a specialized dashboard that gives the users the functionality to visualize the state of the SECLAB and to control their account machines, environment, and states have arisen.

 Currently, the FHICT network managers are working with the default dashboard that comes with the VMware ESXI system Operational Manager. This system can show information on all the needed metrics, but it does not visualize it in an easy to understand way. Additionally, the team does not have the resources to monitor the system themselves, so they need an automated solution such as alerts and suggestions on remediation methods.

### Project Goal

The goal of the project is to improve the performance and problem handling of the Seclab with the help of a Dashboard and automation techniques. 

### Desired project outcome

The stakeholders expressed wishes to have a complete product rather than multiple researched but incomplete or not implemented solutions. For this reason, we opted to focus on dashboard development and its functionalities, with the option if time allows, to research and work on the VPN. 

The project is thereby considered a success if the users can interact with the Seclab via the dashboard; view live and data overtime on the performance, use, behavior on the Seclab; and there is proper documentation on the dashboard use, implementation, and development. 



### Team organization

It has been decided together with the entire team that the method used to coordinate the team will be Agile. The group has decided that communication with the product owner and the mentor will be held weekly to ensure the needs of the project and customer are being met and that the team is heading in the right direction. 



### Preparation Week 1-2

The first 2 weeks were focused on making contact with the shareholders and trying to define our goals for the project. The large number of shareholders coming in contact with all of them posed quite a challenge. The reason we had to meet with the shareholders was to better understand the current situation and the outcome they were hoping to achieve through our project. Through a number of analyzes such as risk analysis, threat analysis, privacy impact assessment, the ethical analysis we were able to come up with the research questions and to confirm them with our shareholders in our first meeting. All project documentation is kept in the [Shared Folder through Teams](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?RootFolder=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project&FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098) .

The shared folder has been reorganized and all minutes of our meetings were designated a [Minutes Folder](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098&viewid=b359bf5d%2D7b13%2D4aa5%2D84c8%2D4b1c859463b6&id=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FMinutes). In addition, the group was organized in such a manner that all members are aware of their role. I was assigned the role of SCRUM Master/project leader responsible for communication with the shareholders and maintaining the assignments for the team.

To this end, we have chosen to use Trello as our main way of keeping track of tasks and goals.

![image alt text](/Seclab-Project/TrelloAgileBoard.png?style=centerme)
##### [Figure 1.1 - Agile board usage] 


After the meeting with the shareholders where we agreed upon our [user requirements](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B8EB77DAB-8ED6-461A-8CDA-3BDED2BFF43C%7D&file=URS%20-%20MoSCoW.docx&action=default&mobileredirect=true&cid=ab2b59d1-38e4-4657-95df-7cf96498a677). In addition to this document I personally worked on the [Ethical analysis](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098&viewid=b359bf5d%2D7b13%2D4aa5%2D84c8%2D4b1c859463b6&id=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FTICT) and on the [Risk assesment](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?RootFolder=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FRiskRannkingAndMitigationTemplate&FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098) together with Alex Petrov and Tymek Angela.

##### Personal Contribution
- Organized meeting with shareholders
- Worked together with the rest of the team on the [user requirements](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B8EB77DAB-8ED6-461A-8CDA-3BDED2BFF43C%7D&file=URS%20-%20MoSCoW.docx&action=default&mobileredirect=true&cid=ab2b59d1-38e4-4657-95df-7cf96498a677) (I came with the document structure)
- I made the [Ethical analysis](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098&viewid=b359bf5d%2D7b13%2D4aa5%2D84c8%2D4b1c859463b6&id=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FTICT)
- [Risk assesment](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?RootFolder=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FRiskRannkingAndMitigationTemplate&FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098) together with Alex Petrov and Tymek Angela.



### Project definition week 3-4

The goal of these two weeks were to crete the [Project Plan](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B84BE18EB-7CA1-4A39-9C4E-F6990B9B5F3B%7D&file=project_plan_v1.docx&action=default&mobileredirect=true&cid=ff07e989-a71d-4143-ae88-0fc2b14a5ccd) based on the user requirements that the Product Owner and the Shareholders agreed on. To this end, all the prior documentation and meeting knowledge was used. 

On top of creating the project plan we also started some of our initial research based on the sub-questions derived from the main research question. Together with Melanie Janssen, we worked on [" What data will be shown on the dashboard and how will it be presented?"](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B58CA3EE7-8AA1-49AC-8F61-B878946F4D06%7D&file=Dashboard%20metrics.docx&action=default&mobileredirect=true&cid=6d4a7864-3eac-42d9-bb45-6c3ff32bc888)

##### Personal Contribution

- Worked along with the rest of the team on the [Project Plan](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B84BE18EB-7CA1-4A39-9C4E-F6990B9B5F3B%7D&file=project_plan_v1.docx&action=default&mobileredirect=true&cid=ff07e989-a71d-4143-ae88-0fc2b14a5ccd)
- Made a research document [research document](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B58CA3EE7-8AA1-49AC-8F61-B878946F4D06%7D&file=Dashboard%20metrics.docx&action=default&mobileredirect=true&cid=6d4a7864-3eac-42d9-bb45-6c3ff32bc888) together with Melanie Janssen 


### Sprint 1, week 5-8 (Sep 28 -Oct 23)

The initial goals for the first sprint were to research authentification to the dashboard, to make a dashboard UI prototype, to define baseline statistics for the metrics, to write several automated PowerShell scripts, and to delve deeper into the research questions. Due to the limitation of the test environment that was harder to set up than anticipated in addition to the requests and change of the PO we had to accordingly change our goals as well.

As a result, the new goals of the sprint became to create PowerCLI scripts based on a list of requirements given to us by the PO
(Product owner), to define the first look of the  Dashboard and to set up the test environment.

My personal assignments during this sprint were to help communicate our problems with the test environment, create a PowerCLI script for the test environment, and start delving into creating a dashboard website.

I helped with the setting up of the server and later on, we decided to move the physical server to a location from Fontys so we could set up an internet connection. Due to the location limitation finishing up the setup of the environment was left to Dane Marc. Afterward, we all worked together to test the connection into the test environment

![Enviroment](/Seclab-Project/Connection_To_TestEnvironment.png?style=centerme)

##### [Figure 1.2 - Test environemnt - vSphere] 

Furthermore, after successfully setting up the server I was able to write a script that enabled finding all the Thick provisioned machines inside our environment and gives a short message to alert the user of this. Due to the absolute lack of knowledge of Powershell, I found the development of this short script very educational and a bit harder than I originally thought it would be.

![Working script](/Seclab-Project/Script_Working.png?style=centerme)
##### [Figure 1.3 - Working script] 

##### Personal Contribution
- Made script for finding all thick provisioned scripts
- Worked with Marc to setup the esxi environment on the servers we received and helped with transporting the servers


### Sprint 2, week 9-12 (Nov 2 -Nov 27)

Based on the Sprint meeting that was held together with the newly assigned PO (Product Owner), Mr. Schellekens Casper, and the network administrators that are responsible for the Netlab environment the team formed newsprint goals.

During the sprint meeting, I personally was responsible for presenting the overall situation of the project and to conduct the conversation with the PO to showcase our current work, clarify our goals, and receive feedback on how the project should proceed.

![Sprint Goals](/Seclab-Project/Sprint2planing.png)
##### [Figure 2.1 - Sprint 2 goals ] 

As sent in Figure 2.1, the team decided on the next sprint goals based on the feedback that was received during the sprint meeting. As SCRUM master I organized the board together with the team and made sure that everyone has clear tasks. 

One of the main concerns during the Sprint 2 meeting was the lack of good dashboard wireframes. Therefore, one of the main goals of the second sprint was to create the dashboard frontend and the graphs that would be displayed in it to showcase it to review it and approve it during the next Sprint meeting.

![Sprint Goals](/Seclab-Project/Activities.png)
##### [ Figure 2.2 - Website making activities]
![Sprint Goals](/Seclab-Project/Activities2.png)
##### [Figure 2.3 - Website making activities]

This task was divided between me and Alex Petrov, and Marc Dane. Because the team did not have a lot of experience with web development we had to spend the first week researching Angular the chosen framework. During the research process, I found a pre-made dashboard solution ngx-admin. This was a dashboard solution with a lot of front-end elements that greatly helped us speed up the process of creating a dashboard.

![Sprint Goals](/Seclab-Project/DashboardUserPage.png)
##### [Figure 2.4 - Dashboard made in Angular]

At the end of the Sprint due to difficulties that came from 2 of our group members quitting the project we only got one out of three pages done. The User page that I was tasked with finishing. The page featured some of the graphs that could be displayed and the overall look of the website.

In addition, the page also featured the Grafana dashboards that were added with the help of Loek Vogels. The look of the dashboard was approved by both the PO and the network administrators of Netlab.

Furthermore, to coordinate between all team members we used a new version control system, Azure Repos. Due to some difficulties, the repo had to be reset a few times but in the end, the team managed to sync all their work. The repository can be found [here](https://dev.azure.com/SeclabRDFontys/_git/SeclabNgAdminDashboardBase). 

If you do not have permission to see the repository contact the repository admin, Gabriel Vlad Luca.

![Sprint Goals](/Seclab-Project/QuesionBord.png)
##### [Figure 2.5 - Question Board]

To keep track of all the questions we had we used a Trello board and the main channel of communication was Teams as emails seemed to be too slow and daily meetings were not possible due to the Corona situation.


![Sprint Goals](/Seclab-Project/FormSurvey.png)
##### [Figure 2.6 - Survey for dashboard graphs and information]
At the end of the sprint at the suggestion of the PO, I worked on a Survey to better understand the metrics that were needed by the users of Netlab. The survey was distributed with the help of the PO to students in the cyber specialization, to infrastructure students, and cybersecurity minor students. In total 21 individuals participated in the survey.

The goal of the survey was to find additional graphs and information to add to the dashboards. This goal was achieved and in addition, the survey participants also came with unique ways to visualize and take advantage of the data that we collect for this project.

In addition to working on the dashboard and survey as the group leader I was tasked with the communication between the group and the PO, I was responsible for communicating our meetings, questions, and necessary resources to conduct the project.

##### Personal Contribution
- Made Angular Dashboard.
- Wrote, distributed and analyzed survey for dashboard.
- Organized stand-up meetings.

### Sprint 3, week 13-17 (Nov 30 -Jan 18)

At the end of the second sprint another sprint meeting was conducted. The discussed points can also be found in the [minutes](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/FHICT%20research%20and%20development%20project/Minutes/Notes_26_11_2020.txt). All minutes for all meetings conducted can be found in the teams [shared folder](https://teams.microsoft.com/_#/school/files/FHICT%20research%20and%20development%20project?threadId=19%3A5d69e2a976384993af588f7e4a8c8ca3%40thread.skype&ctx=channel&context=Minutes&rootfolder=%252Fsites%252FCyberSecurityMinor20192020-FHICTresearchanddevelopmentproject%252FGedeelde%2520documenten%252FFHICT%2520research%2520and%2520development%2520project%252FMinutes)

During the 3rd sprint meeting, I presented the updates on the Dashboard and the Survey results. Overall the shareholders that were present at the meeting were very pleased with the progress that was made for this sprint.

As a result of the feedback they gave the team we decided that the most important goal for the 3rd and last sprint was to deliver working dashboards based on the survey and needs of the shareholders. On top of that suitable documentation should come with all deliverables. Finally, they wanted us to integrate our scripts and our test environment machines inside the Netlab environment.


![Sprint Goals](/Seclab-Project/sprint3board.png)
##### [Figure 3.1 - Sprint 3 goals]
To keep track of the sprint goals we used the same method as before, Trello boards. Because 2 of the group members left we had to split these 3 tasks between the 5 remaining members. The website would be done by Loek, Marc, and I and the scripts would be finished by Melanie and Koen. 



![Sprint Goals](/Seclab-Project/Sprint3tasks.png)
##### [Figure 3.2 - Sprint 3 weekly tasks]
To keep track of the weekly tasks we conducted weekly meetings where I as the scrum leader together with the other members would decide how we would break down the major deliverable goals into smaller weekly assignments that we could track.

Each task was assigned to the respective team member that is responsible for it and the time limit so we would keep the team focused on the deadlines. On top of this, the reason why we chose to keep track of the work through Trello boards is that it is easy to visualize the workload each member of the team participated with.


During this sprint I continued work on the website but since it was decided that we will not have enough time to develop and implement it I switched my work on researching how to improve the Grafana dashboard to that end I researched how to add an identity server and I worked together with Loek to add alarm functionality on Grafana.

![Sprint Goals](/Seclab-Project/TeamButton.png)
##### [Figure 3.3 - Team communication feature]
At the beginning of the project, one of the proposed features for the dashboard was an issue board or a communication medium between the students and the network administrators. Because in the later sprints the network administrators decided that this feature is no longer needed as they have enough communication channels we decided to add the current and most used one to the website.

Originally we tried to add Teams directly to the website as a complete chat function but what was asked of us was just a connection to the current channel the "FHICT Netlab club". To achieve this we had to research different ways to link Teams through the website and how to use the Websites elements (the button in this case). In conjunction with the method for the website.

![Sprint Goals](/Seclab-Project/TeamCom.png)
##### [Figure 3.4 - Netlab Communicaiton channel]
It was fully working but sadly the website deliverable had to be scraped and used just as research because the team decided together with the Product Owner that there is not enough time to fully develop, test, and implement the website due to the constraints that appeared with communicating with the network administrators and a few of our colleagues leaving the group.


![Sprint Goals](/Seclab-Project/GrafanaGraphsInAngular.png)
##### [Figure 3.5 - Addmin Alarms into the website]
At the beginning of the sprint, before it was decided that the website should not be used as the main dashboard I also added the functionality needed to have the Graphs from Grafana used directly into the website. This proved to be a more useful and direct way to use the graphs over sending the data from Telegraf to our custom made graphs and interpreting the data ourselves.

![Sprint Goals](/Seclab-Project/GrapgAlarms.png)
##### [Figure 3.6 - Adding alarms to the Graphs]
Once it was decided that the website solution cannot be continued I had to change my focus onto the Grafana dashboard. Which at this point became the main dashboard that we would deliver. Because it was set up by Loek I worked together with him to set up the alarms on the Grafana website and also researched how to implement the remaining functionalities for the dashboard directly in Grafana. The aforementioned functionality was alerting that we already implemented and logging using the credentials of the students. 

![Sprint Goals](/Seclab-Project/IdentityServer.png)
##### [Figure 3.7 - Grafana identity server configuration]
One solution that I came up with as a result of the research was linking the identity servers used by the network administrators to hold the Netlab credentials as a base to login to the Grafana website. Afterward, together with Loek I found the configurations and documented the steps necessary to implement this functionality.

![Sprint Goals](/Seclab-Project/IdentityServer2.png)
##### [Figure 3.8 - Grafana identity server configuration]
Sadly the configuration that we had did not work because the credentials that we got for our account inside the Production environment did not have sufficient credentials but with the proper credentials from the network administrators it would have worked.



On top of that I worked on making the [dashboard  research document](https://teams.microsoft.com/l/file/EEE950C3-1FD6-4252-8CA1-BE104011B9D6?tenantId=c66b6765-b794-4a2b-84ed-845b341c086a&fileType=docx&objectUrl=https%3A%2F%2Fstichtingfontys.sharepoint.com%2Fsites%2FCyberSecurityMinor20192020-FHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FDashboardResearchDocument.docx&baseUrl=https%3A%2F%2Fstichtingfontys.sharepoint.com%2Fsites%2FCyberSecurityMinor20192020-FHICTresearchanddevelopmentproject&serviceName=teams&threadId=19:5d69e2a976384993af588f7e4a8c8ca3@thread.skype&groupId=03769ec5-ba87-41ba-add8-d725dfd80e35). This document is used by the network administrators to understand how the dashboard works and what research went into the current solution.

##### Personal Contribution
- Added Grafana graphs to dashboard
- Added team communication funtionality
- Worked with Loek to add Graph alarms
- Researched identity server implementation for Grafana
- Made dashboard [dashboard  research document](https://teams.microsoft.com/l/file/EEE950C3-1FD6-4252-8CA1-BE104011B9D6?tenantId=c66b6765-b794-4a2b-84ed-845b341c086a&fileType=docx&objectUrl=https%3A%2F%2Fstichtingfontys.sharepoint.com%2Fsites%2FCyberSecurityMinor20192020-FHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FDashboardResearchDocument.docx&baseUrl=https%3A%2F%2Fstichtingfontys.sharepoint.com%2Fsites%2FCyberSecurityMinor20192020-FHICTresearchanddevelopmentproject&serviceName=teams&threadId=19:5d69e2a976384993af588f7e4a8c8ca3@thread.skype&groupId=03769ec5-ba87-41ba-add8-d725dfd80e35)