# DigiBP Gimmelwald HR Recruitment [![License](http://img.shields.io/:license-apache-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html) [![Deploy to Heroku](https://img.shields.io/badge/deploy%20to-Heroku-6762a6.svg?longCache=true)](https://heroku.com/deploy)
This repository contains the documentation and digitalized HR recruitment process running on [Camunda BPM](https://docs.camunda.org). This repository is part of the group assignment of the Digitalisation of the Business Processes (SS19) Module at the FHNW and is based on the [digibp-camunda-template](https://github.com/DigiBP/digibp-camunda-template) (Version 3.2.3)

*Authors*
* Robert Johner, [robert.johner@students.fhnw.ch](mailto:robert.johner@students.fhnw.ch)
* Ragesh Chellathuray, [ragesh.chellathuray@students.fhnw.ch](mailto:ragesh.chellathuray@students.fhnw.ch)
* Rebecca Ganz, [rebecca.ganz@students.fhnw.ch](mailto:rebecca.ganz@students.fhnw.ch)
* Maurus Hodel, [maurus.hodel@students.fhnw.ch](mailto:XXX.XX@students.fhnw.ch)
* Silvan Trifari, [silvan.trifari@students.fhnw.ch](mailto:silvan.trifari@students.fhnw.ch)


## This readme is structured as follow:
1. Introduction
2. Process flow - as is
3. Process flow - to be
4. HR recruitment process
5. Subprocess preliminary sounding Part 1
6. Subprocess preliminary sounding Part 2
7. Subprocess candidate interview
8. Subprocess offer negotiation
9. Subprocess onboarding
10. Process Description
11. Service Integration
12. Typical Challenges
13. Project Challenges
14. Recommendations


## Introduction
As part of the Digitalization of Business Processes module, Team Gimmelwald decided to focus on the HR recruitment process and to digitize that process. In the following paragraphs the recruitment process for a new position, any used artefacts as well as integrated services (if any) are described.

### Process flow - as is
Before we started to digitalize the recruitment flow, we started with modelling a draft of the process as is it is. This is how we modeled it:
![digibp-template](https://user-images.githubusercontent.com/47921658/58194575-340ac280-7cc6-11e9-9bd6-fa502a6646e1.png)
The process starts at at the business unit manager, which detects a vacancy and makes a specification for it. The HR department asses the vacancy and requests an approval. If the request is denied the process ends. If it is approved, a new job case is opened and the job description and advertisement is written. The advertisement goes to the BU manager who checks and releases it.
After some time, the first candidates will send their application documents to the HR department. They collect all applications and check them: if a candidate is not fitting for the recruited position, an rejection letter is send. If the candidate is fitting, HR proposes a time slot for a meeting by phone.
As soon as the time slot is confirmed by the candidate the HR department terminates a meeting with the BU manager and the candidate. After this call, the BU manager decides if the candidate is selected or not.
The second round is identical to the first interview, except that the meeting is hold in person on premise. If the BU manager selects the candidate, HR makes an job offer.
After the candidate received the offer there are 3 possibilities:
1. The candidate directly accepts
2. The candidate directly calls of
3. The offer is negotiated between the company and the candidate

If the offer is excepted, the HR department creates an employment contract.
As soon as the contact is signed and returned, the HR informes the BU manager, requests the preparation of a new workspace and all system accesses.
The process ends with the welcome of the new employee to his first day of work.

### Process flow - to be
After we described the "as is" process we started to digitize the process to its future form. This "to be" process will be described in the following paragraphs.
For this project, the scope of the HR recruitment process has been defined to start with the request for a new employee to join the team, and ends when a new employee is successfully hired. The affected departments (e.g. Business unit or HR department) are labeled and the assigned with corresponding tasks. The process scope ends with the part when the new employee signs and returns the contract and furthermore the onboarding process has been initiated. The onboarding itself and any trainings or the first day of the new employee is not part of this process scope.
Camunda modeler was used to model the HR recruitment process. The main HR recruitment process also contains individual processes which are modelled as subprocess. Those are called in the main HR recruitment process. Once the called subprocess is completed, token returns to the main HR recruitment process and continues with the modelled tasks. The digitalization of process is focused on the subprocess "preliminary sounding" due to most value added on the process flow.

### HR recruitment process
![HR Mainprocess](https://user-images.githubusercontent.com/47921658/58235796-d451fd00-7d41-11e9-91e5-5d8f755f279e.png)
This is the main HR recruitment process which includes all the steps starting from vacancy definition until the onboarding steps for the new employee.  In this process the actors from the business unit where the vacancy has been identified (e.g. Business Unit Manager) and someone from the HR department (e.g.  HR Recruiter). This process also includes also includes subprocess which are initiated from this main process. Each subprocess is also individually explained in the following chapters.

The business unit identified that one or several tasks needs to be done by a person which is currently or in the near future (i.e. someone leaving) not staffed. Based on this circumstance the Business Unit Manager defines the vacancy. For vacancy creation rules and regulations defined by the HR department needs to be considered to be compliant. Once the vacancy for the affected departement is defined as a next step someone from the HR department will step-in to assess the vacancy. Based on this assessment either a new job case including job description and advertisement will be created or the vacancy will be closed. In case of a creation of job advertisement, this will go to the business unit department for a final check. prior publishing the advertisement on the company webpage by the HR Department (artefact).

Now an intermediate is included which is awaiting the defined deadline date for the incoming applications. Once deadline date has been arrived the subprocess preliminary sounding is called.


##### Subprocess preliminary sounding Part 1
![Subprocess Preliminary Sounding Part 1](https://user-images.githubusercontent.com/47921658/58236542-64447680-7d43-11e9-8e18-eb939062ddd6.png)

The subprocess preliminary sounding part 1 is called by HR main process task “call subprocess preliminary sounding”. The called subprocess starts once the application deadline expires. The first tasks collects each incoming applications which was by then collected in the application database. Next step is to check the application. For this purpose task “check applications” calls the subprocess “preliminary sounding part 2” (see chapter “Subprocess preliminary sounding part 2”). The outcome from the part 2 is used for human decision which decides whether application should be rejected or the candidate should be invited for an interview. In case of a rejection a corresponding email is sent to the applicant. Otherwise after a final manual check potential time slots will be gathered and timeslot suggestions will be sent to the applicant.

##### Subprocess preliminary sounding Part 2
![Subprocess Preliminary Sounding Part 2](https://user-images.githubusercontent.com/47921658/58236150-8b4e7880-7d42-11e9-8136-05951c30b65f.png)

The subprocess preliminary sounding part 2 is called by task “check applicants” which is called in subprocess preliminary sounding part 1.
Once the deadline has been passed - until then the incoming applications are stored in a database within the company - the integrated service from RezScore is called to rate the incoming applications. For this purpose the CVs will go through the external application which scans the skills and experiences stated in the CV. As an output each individual CV resp. application will be rated between A and F (A== full fit; F==no fit) and added in a spreadsheet.

##### Subprocess candidate interview
![Subprocess Candidate interview](https://user-images.githubusercontent.com/47921658/58236182-9b665800-7d42-11e9-9eaa-fbc1dda8d66e.png)

Once the preliminary sounding has been closed as a next step the candidate interview subprocess will be called from the main process. Within this subprocess the starting event awaits the confirmed time slot for the interview from the candidate. Afterwards the time slot will be again confirmed by HR department with the candidate and the business department. The next step is the interview between one responsible in the business unit and the candidate. Based on this interview the business unit has to decide whether the candidate is still interesting for the company or not. If the latter is the case the candidate will be rejected. Otherwise HR will be notified that candidate is still an option for the business unit. This subprocess will be passed through also for the 2nd interview session. There is no interaction with other services needed for this subprocess. Therefore no service integration required in this subprocess.

##### Subprocess offer negotiation
![Subprocess Offer Negotiation](https://user-images.githubusercontent.com/47921658/58236200-a7eab080-7d42-11e9-9d3b-006336638955.png)

After the 2nd interview the business unit needs to decide for the suitable candidate. Once the potential new employee has been chosen, an offer is send to the candidate. As a next step the subprocess offer negotiation is called. On basis of the candidates feedback the offer is accepted, rejected or requires further negotiations. If the offers is rejected the process ends with the rejections. If the offer is accepted the HR department creates an employment contract and sends the contract to the candidate. In case of a negotiation there is an interim step which either ends with an offer rejection or offer acceptance.
There is no service integration required in this subprocess.

##### Subprocess onboarding
![Subprocess Onboarding](https://user-images.githubusercontent.com/47921658/58236218-b5079f80-7d42-11e9-8c92-a6f4def3f0ef.png)

As the final step in the main HR recruitment process the subprocess onboarding is called. Within this subprocess the starting event is triggered once the signed contract has been obtained. The returned contract initiates three tasks in parallel which notifies the Business Unit managers about the new team member and the requests for the workplace installation and new user account. As a last step within this subprocess the candidate will be welcomed on his or her first day of work. With this final task also the main HR recruitment process ends.
There is no service integration required in this subprocess.

## Process Description
### Roles
Different roles are involved to execute the process. For an easier understanding, we will split the roles in the two categories "internal" and "external".

#### Internal Roles
- Business Unit Manger:
The Business Unit Manager is starting our recruitment process by specifying the vacancy requirements, directly after the vacancy has been detected in his business unit. This role is involved in sever steps during the recruitment process and participates in the major decisions. The Manager is the direct supervisor of the future employee.
- HR department:
Our HR department has an integral role during the recruitment process. HR is responsible for most of the task and works as an intermediary between the business unit manager and the candidates.

#### External Roles
- Candidate:
The Role Candidate is represtented by applicants that are interested in the vacancy and therefore apply for the job. This role can be represented by one or more applicant.


# Service Integration
Some tasks of the described modeled HR-process includes service integration. That means the tasks are executable within the process automatically. Therefore services access data from different different IT systems and brings them together to optimize and digitalize the process. The different used IT systems in our case are described below:

### Tools Used
- **Camunda BPM Modeler**: The Camunda BPM Modeler is a desktop application used for modeling the HR recruitment process in BPMN. 
- **Camunda Platform**: The Camunda Platform is a opensource platform which allows the technical execution of the designed process with the Camunda BPM modeler. The whole workflow of the HR-recruitment process is executable through the Camunda Platform.
- **GitHub**: GitHub is a online tool to document and model project artefacts and to share the project within the group.
- **Heroku**: Heroku isa cloud application used to deploy the Camunda BPM HR-Process in a platform as a Service (PaaS) environment.
- **Postman**: Postman is a free app used to test the different webservice and send data from Postman to Integromat.
- **Integromat**: Integromat is an online automation platform used for the automation of the process and create customer webservices.
- **Dialogflow**: Dialogflow is the tool for the integration of the Google Assistant Chatbot into the process for a more personalized function for the applicant.
- **Google Forms**: Google Forms is a used to create forms and sharing information through forms between the applicant and the HR.
- **Google Forms Addon: Dynamic Fields**: The Dynamic Fields is an addon to Google forms to create dynamic elements in a Google Form 
- **Google Sheets**: Google Sheets allows to store the entered data online from the linked Google Form.
- **RezScore**: RezScore is a tool to evaluate and classify a cv in the HR-Process with AI and machine learning technology

### Google Sheets
A central Google Sheet is used as database for all the candidates. As candidates are mostly temporary data a Google Sheet is perfect to store this data in it. As soon they get hired an internal account will be created and the applications data is transfered to that system. The Google Sheet can be cleared afterwards. So we can save time and ressources using a simple Google Sheet instead of creating a complex database which needs to be maintained by the IT.
This is how our Google Sheets for the candidates looks like:
![image](https://user-images.githubusercontent.com/17927946/58703382-d43ca780-83a8-11e9-80ae-f2c8843ba10b.png)

### Google Forms
Different Google Forms were used to interrogate with the candidate. Google Forms offers a seamless integration of other Google apps like tge Google Drive office suite along with Google Docs, Google Sheets, and Google Slides. Google Forms features all of the collaboration and sharing features found in Docs, Sheets, and Slides and the information is then collected and automatically connected to a spreadsheet.
Therefore we used Google forms two major interfaces between us and the candidate:

##### Job application
This Google Form is used by the candidate to apply for a specific job. He enters his personal information, chooses the job he wants to apply for and attaches his CV. This informations are then send to the backend where they are stored until the deadline of the specific job is reached and are then processed and assessed.
![googleform1](https://user-images.githubusercontent.com/17927946/58703127-ef5ae780-83a7-11e9-8740-e8469fc5ff95.png)

##### Check applicants
This Google Form is used to accept or reject candiadates where the RezScore API returned a C or D value. All the candidates where are not sufficent for the first telephone call are listed here and can be choosen if they will be invited to the first call or not.
![googleform2](https://user-images.githubusercontent.com/17927946/58703159-0dc0e300-83a8-11e9-98ad-b1ba21199863.png)

##### Google Forms Addon - Dynamic Fields
The Dynamic Fields is an addon to Google forms to create dynamic elements in a Google Form. It allows Google Forms to be rendered dynamically when content in source document changes. This addon is used to dynamically adjust the open jobs in the Google Form to apply for a job:
![image](https://user-images.githubusercontent.com/17927946/58703795-2a5e1a80-83aa-11e9-9afe-898581c1be2c.png)

##### RezScore
The Rezscore aPI provides programmatic services for application documents. It is able to quantitatively analyze application documents, for example CV's. IT allows to rate and compare the documents. The free trial allows us to rate the document and put them in different categories. The full version of Rezscore is be able to classify cv's into set of pre-definied skills. So  it can rate the cv to definied skills for the job and better evualate if the candidate is suitable.
![image](https://user-images.githubusercontent.com/17927946/58703940-a6f0f900-83aa-11e9-8995-9deb6624a1b9.png)


##### Integromat
Intergromat was used to create and launch different webservices from Cammunda.
##### Collect applicants
![image](https://user-images.githubusercontent.com/17927946/58704689-24b60400-83ad-11e9-9e81-9e54ff679834.png)
First it waits the for the corresponding webhook, then it filters all the applicants out where the CV is not yet rated. After that it checks if a CV needs to be rated or not. If its not the case, the webservices returns early. If a cv needs to be rated the corresponding cv file is fetched from Google Drive and then send as parameter to the RezScore API. After that it parses the JSON answer to XML and updates the score in the Google Sheet.


##### Dialogflow
Dialogflow provides an AI powered interaction with your product through text and conversation for example in apps or chatsbots. The connection is possible for example through Amazon Alexa, Google Assistant or Facebook Messenger. It incorporates with Google machine learning expertise and runs on the Google cloud platform and enabels an natural and rich conversation.
The chatbot is used to apply directly for a job with the Google Assistant
![image](https://user-images.githubusercontent.com/17927946/58704433-4498f800-83ac-11e9-9374-2125442a0d41.png)

## Typical Challenges
By the start of the project, our team quickly agreed on the topic of the HR recruitment processes. The reason behind our decision was that we commonly agreed, that the recruitment process consists of many decisions and collaborations with internal and external roles and has high potential of digitalization and automatization. The process is in general often very time consuming as the sounding of application documents can already be messy, simply by the large number of candidates that are applying. Also, the process normally requires more than one round of interviews until the prefered candidate has been found.

### Project Challenges
The first challenge in this project was to determine a standard recruitment process, while making assumptions about the size and type of company that the process is embedded within. We assumed that we are dealing with a mid-size Swiss company with a small HR department and no existing automation or digitization of processes. Following this, a second challenge was to get the process into an executable state, and of course, to integrate certain services. Due to the limited scope of this assignment, we focussed our digitization on one particular subprocess, where we saw the greatest potential for adding value through automation. Many of our initial ideas were not feasible, as they were too complex for our collective skill level, but we agreed on the digitization of the initial CV review subprocess due to its time-intensive nature if done manually.

A further challenge that we struggled with was making certain subprocesses able to instantiate multiple times, therefore allowing several applicants to be considered and move through the recruitment processes at the same time for the same role. We were able to solve this issue by using 'call activities' that refer to separately designed processes that may be called on multiple times.

Overall, despite the challenges of designing and digitizing the process, we have found a functional and efficient solution.

### Recommendations 

As further phases to this digitalization project we recommend analyzing the remaining subprocesses and integrating them into the digitized process flow. For example, interview arrangements and notifications of the interview outcomes by the business unit manager to HR could be integrated in the digitized process and stored in a central repository, potentially as metadata attached to the candidates’ application files. Furthermore, it could be analyzed whether an integration of the recruitment process and files with the employee data of the successful candidates would be valuable. Finally, this process only considered the onboarding process briefly and on a very high level. A thorough analysis and detailed modelling of the whole onboarding process, from receipt of the signed employment contract up to and including a new employee’s first working day, induction and training, should be considered in a future project phase. 


## License

- [Apache License, Version 2.0](https://github.com/DigiBP/digibp-archetype-camunda-boot/blob/master/LICENSE)

