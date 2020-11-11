---
title: "Seclab-Project"
date: 2020-11-05T13:10:54+01:00
draft: true
---

### Project Description
 Fontys ICT is interested in an improved version of their SECLAB virtual environment that provides a solution for monitoring, visualizing and interacting with the SECLAB environment in a more secure and user-friendly way.

 Now Fontys ICT is experiencing difficulties with visualizing the global state of the environment and have a delay in response to problems as they cannot find the proper remediation for it in time. Based on this and the demand from teachers, students and administrators the need for a specialised dashboard that gives the users the functionality to visualize the state of the SECLAB and to control their account machines, environment and states have arisen.

 Currently, the FHICT network managers are working with the default dashboard that comes with the VMware ESXI system Operational Manager. This system can show information on all the needed metrics, but it does not visualize it in an easy to understand way. Additionally, the team does not have the resources to monitor the system themselves, so they need an automated solution such as alerts and suggestions on remediation methods.

### Project Goal

The goal of the project is to improve the performance and problem handling of the Seclab with the help of a Dashboard and automation techniques. 

### Desired project outcome

The stakeholders expressed wishes to have a complete product rather than multiple researched but incomplete or not implemented solutions. For this reason, we opted to focus on dashboard development and its functionalities, with the option if time allows, to research and work on the eduVPN. 

The project is thereby considered a success if the users can interact with the Seclab via the dashboard; view live and data overtime on the performance, use, behaviour on the Seclab; and there is proper documentation on the dashboard use, implementation and development. 



### Team organization

It has been decided together with the entire team that the method used to coordinate the team will be Agile. The group has decided that communication with the product owner and the mentor will be held weekly as to ensure the needs of the project and customer are being  met and that the team is heading in the right direction. 



### Preparation Week 1-2

The first 2 weeks were focused on making contact with the shareholders and trying to define our goals for the project. Due to the large number of shareholders coming in contact with all of them posed quite a challenge. The reason we had to meet with the shareholders was to better understand the current situation and the outcome they were hoping to achieve through our project. Through a number of analyzes such as risk analysis, threat analysis , privacy impact assessment, ethical analysis we were able to come up with the research questions and to confirm them with our shareholders in our first meeting. All project documentation is kept in the [Shared Folder through Teams](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?RootFolder=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project&FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098) .

The shared folder has been reorganized and all minutes of our meetings were designated a [Minutes Folder](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098&viewid=b359bf5d%2D7b13%2D4aa5%2D84c8%2D4b1c859463b6&id=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FMinutes). In addition, the group was organized in such a manner that all members are aware of their role. I was assigned the role of SCRUM master/project leader responsible for communication with the shareholders and maintaining the assignments for the team.

To this end we have chosen to use Trello as our main way of keeping track of tasks and goals.

![image alt text](/Seclab-Project/TrelloAgileBoard.png?style=centerme)
##### [Figure 1.1 - Agile board usage] 


After the meeting with the shareholders where we agreed upon our [user requirements](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B8EB77DAB-8ED6-461A-8CDA-3BDED2BFF43C%7D&file=URS%20-%20MoSCoW.docx&action=default&mobileredirect=true&cid=ab2b59d1-38e4-4657-95df-7cf96498a677). In addition to this document I personally worked on the [Ethical analysis](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098&viewid=b359bf5d%2D7b13%2D4aa5%2D84c8%2D4b1c859463b6&id=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FTICT) and on the [Risk assesment](https://stichtingfontys.sharepoint.com/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/Gedeelde%20documenten/Forms/AllItems.aspx?RootFolder=%2Fsites%2FCyberSecurityMinor20192020%2DFHICTresearchanddevelopmentproject%2FGedeelde%20documenten%2FFHICT%20research%20and%20development%20project%2FRiskRannkingAndMitigationTemplate&FolderCTID=0x012000F78CEB40F297C044A67ABC914FF42098) together with Alex Petrov and Tymek Angela.


### Project definition week 3-4

The goal of these two weeks were to crete the [Project Plan](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B84BE18EB-7CA1-4A39-9C4E-F6990B9B5F3B%7D&file=project_plan_v1.docx&action=default&mobileredirect=true&cid=ff07e989-a71d-4143-ae88-0fc2b14a5ccd) based on the user requirements that the Product Owner and the Shareholders agreed on. To this end all the prior documentation and meeting knowledge was used. 

On top of creating the project plan we also started some of our initial research based on the sub-questions derived from the main research question. Together with Melanie Janssen we worked on [" What data will be shown on the dashboard and how will it be presented?"](https://stichtingfontys.sharepoint.com/:w:/r/sites/CyberSecurityMinor20192020-FHICTresearchanddevelopmentproject/_layouts/15/Doc.aspx?sourcedoc=%7B58CA3EE7-8AA1-49AC-8F61-B878946F4D06%7D&file=Dashboard%20metrics.docx&action=default&mobileredirect=true&cid=6d4a7864-3eac-42d9-bb45-6c3ff32bc888)

### Sprint 1, week 5-8 (Sep 28 -Oct 23)

The initial goals for the first sprint where to research authentification to the dashboard, to make a dashboard UI prototype, to define baseline statistics for the metrics, to write a number of automated powershell scripts and to delve deeper into the research questions. Due to the limitation of the test environment that was harder to set up than anticipated in addition to the requests and change of the PO we had to accordingly change our goals as well.

As a result the new goals of the sprint became to create PowerCLI scripts based on a list of requirements given to us by the PO
(Product owner), to define the first look of the  Dashboard and to set up the test environment.

My personal assignments during this sprint where to help communicate our problems with the test environment, create a PowerCLI script for the test environment and to start delving into creating a dashboard website.

I helped with the setting up of the server and later on we decided to move the physical server to a location from Fontys so we could set up a internet connection. Due to the location limitation finishing up the set up of the environment was left to Dane Marc. Afterwards we all worked to together to test the connection into the test environment

![Enviroment](/Seclab-Project/Connection_To_TestEnvironment.png?style=centerme)

##### [Figure 1.2 - Test environemnt - vSphere] 

Furthermore after successfully setting up the server I was able to write a script that enabled finding all the Thick provsioned machines inside our environment and gives a short message to alert the user of this. Due to the absolute lack of knowledge of Powershell I found the development of this short script very educational and a bit harder than I originally thought it would be.

![Working script](/Seclab-Project/Script_Working.png?style=centerme)
##### [Figure 1.3 - Working script] 

