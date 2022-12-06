# Jenkins
VmTutes Jenkins
Jenkins:
========

Introduction to jenkins
Installation
Archetecture
master and salves configurations
what is job
creating jobs
scheduling jobs
labels
dependencys
Continuous Integration
maven project 
Continuous Deployment
Email Notifications
sonarcube
jfrog
creating users
creating roles
backups
list and nested views
pipeline
best-pratices
Blue Occean
CatLight Notifier

Without jenkins:- usually when all the developers commits the code to repository(git), at the end of the day QA collects
Integrate all the code and start testing (also called nightly builds), if any bugs found they've to wait up to next day morning to report to developer.

With jenkins:-  whenever dev commits, instantly it integrate the changes and test(unit) it (hardly it takes 10-15minites to report to dev)

Benefits of CI/CD :- @ immediate bug deduction
		 @ Minimal workflow:- integrating the latest changes and testing, if we want to continue like deployment and other steps...like pkg, deployÖetc Go head 
		 @ we can deploy at any point of time
		 @ record the build history for tracking
                 @ speed delivery 
                 
List of CI/CD Tools:
-------------------
 	*Hudson (Enterprice Licenced Tool)
	*Jenkins
 	*Buildforge, 
	*Travis CI, 
 	*Go CD,
 	*Continum, 
	*Anthi pro, 
 	*Circle ci, 
 	*Code fresh, 
 	*Cruse control,
	*bamboo,
	*teamcity
# Hudson and Jenkins Both are same
  - jenkins derived from hudson
  - koshuke kawagachi(open source community developer)   (ecllipse=> sun microsystems (oracle))

Why jenkins?
     open source
     continuous integration
     continuous deployment
     jenkins has thousands of plugins which is used to connect to other tools also
     jenkins is  a frame work( you chose what process you want and ask jenkins to do)
     jenkins Acts as crontab(jobs), job schedules.

Prerequisites:
-------------
Java openjdk 11 or 17 should be installed

Installation:
-------------
* Goto jenkins official website: https://jenkins.io/
* Click on Downloads
* Click on Windows under Long-term Support(LTS) session.
* Unzip the folder(jenkins-2.138.2) and install 
      Note:- while installing jenkins redirect to any of D:/E:/F:/G:/ drive instated of installing in default C:/ drive
* Now open any of your web browser and type  http://localhost:8080
* It will ask for Unlock Jenkins by giving Administrative Passward, in my case i installed in D:/ Drive (D:\jenkins 2.138.2\secrets\initialAdminPssword)
* Click on Continue and select Install Suggested Plugins
* Create First Admin User
* Click on Start using Jenkins 
              (OR)
* Install Jenkins through CLI(command line interface)
* Navigate to jenkins.war file in (D:/jenkins 2.138.2)
* And give this command   jave -jar jenkins.war
     Note:- If you want to change port for jenkins,then run jenkins on another port(9090) by giving this command
             jave -jar jenkins.war --httpPort=9090 in jenkins.war file navigation.

Jenkins Installation in EC2
---------------------------
Prerequisites:-
---------------
1. Launch Jenkins server in ec2
2. need Slave server with Java installation
Note:-  Master and slave should have same Java version

Java Installation
-----------------
1. yum install java-17-openjdk java-17-openjdk-devel -y
  java -version
  which java
  whereis java
  ls -l /usr/bin/java
  ls -l /etc/alternatives/java
  java path --->> /usr/lib/jvm/java-17-openjdk-17.0.5.0.8-2.el8_6.x86_64/bin/java

2. https://pkg.jenkins.io/redhat-stable/
3. sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
4. sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
5. sudo yum install jenkins -y
       Note:- /var/lib/jenkins -->> jenkins installed place
6. sudo systemctl start jenkins (or) sudo service jenkins status
7. open port in security groups
     select security groups link->inbond rule->select rule->add 8080
    a. Select Add Rule, and then select HTTP from the Type list.
    b. Select Add Rule, and then select Custom TCP Rule from the Type list. Under Port Range, enter 8080.
     note:- https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/#prerequisites.
     Note:- for screenshort go to Jenkins installations folder in google drive.
8. open browser and give (ec2 public ip)3.14.29.65:8080

To unlock jenkins:- sudo vi /var/lib/jenkins/secrets/initialAdminPassword

Note:- not to run many job in master
       https://www.jenkins.io/doc/book/security/controller-isolation/
Note:- vi /var/lib/jenkins/config.xml -->> jenkins version
Note:- vi /etc/sysconfig/jenkins -->> to change the port


Archetecture:
------------
10 projects
2hrs each to build
only we have 7 hrs to build

#code integration:

	project m1, m2, m3...m100
	no of issures will arise = 10000
	we cannot predection of integration issures

|--|--|--|--|-------------|
 m1 m2 m3
m1=10issues, m2=10issues, m3 10issues  (1 hr_integrate + 7 hr_dev)
.
.
.
finally only 10 issures will be there

Configurations:
---------------
Global Tool Configuration
 >>Tools
 >>Environmental Variables
Job Configuration
 >>where to run
 >>when to run
 >>what exactly to run
Node Configuration
Master Configurations
Plugin Configuration


# Jenkins is client and server architecture, but no need to install Jenkins in both sides, only one i.e. Jenkins master server.
 
Note:- if we not configure(specify) any node, then the jobs will run by default in jenkins master server where Jenkins  installed.

1. Jenkins master server (or) Build-in Node:-
---------------------------------------------

Global Configuration( Go to Manage jenkins ---> Configure system)
  - global master settings giving to node
@ System Message:- is a Banner/User message who ever login as jenkins user
@ # of executions:- How many jobs a node can take and it depends on the hardware
      and nodes on which you running. (cpu and mem utilization).
@ label:- group of servers
@ Usage:- if we not configure(specify) any node, then the jobs will run by
      default in jenkins master server where Jenkins  installed.
	E.g. if we want to get minimal things like time, mem usageÖetc
@ Quiet Period:- before executing particular task Jenkins put job on hold(wait)
       for 5 sec e.g. any network issuesÖetc
@ SCM checkout retry count:- if jenkins is not connecting to any SCM Tools then it
     retries to connect.

  - environment variables
 -  Build tools info
 -  scm tools

2. Node (servers) Configuration
Node:-
----
Remote root directory  /opt/vmtutes

Launch agent by connecting it to the controller : - we are going to run commands on slave machine to connect master machine.
Launch agent via execution of command on the controller :- run the commands on master to connect to slave.

note:- Use WebSocket to connect to the Jenkins master

Jenkins Official Doc Links:- https://www.jenkins.io/doc/book/managing/nodes/
                             https://www.jenkins.io/doc/book/    
Lessons Learned:-
Same Maven version
 i. In case Jave or Maven paths are referring to the agent system then aline it according to masters Global Tool Configuration
 ii. In case the case of the AWS server make sure your Jenkins URL should be updated to the latest Public IP.

3. Job---> Group of tasks
	       - What
	       - How
	       - When
Note:- By default Jenkins run continuous integration

Creating Jobs:-
This project is parameterized: passing parameters as values
Discard old builds : logs
Restrict where this project can be run : in which slave the job should run

Labels
------
if jobs are running in one server, all of sudden it went down then the jobs in that server will not run.so, to overcome this
	    in jenkins by grouping the servers  and label them with a name and assign jobs.
Ex:- 
		Server x   							
		job 1			MY_SERVERS
		job 2			centos	       
		job 3			febora
                                        Redhat
                                        ubuntu	       
		Server y
		job 1
		job 2
		job 3

CI:
--

Jan|---------|-------|--------------------|------|--------|July
   RA        DE      feb    devphase      may    QA     prod 


# devops guy is responsible to perform a CI in a project
  -- Repository(scm/svn) : Poll Scm
  -- integrating the changes
  -- build(Incremental build)
      >> resource
      >> compile
      >> test
      >> package
      >> install
      >> deploy
      >> code coverage
      >> Static code analysis    
  -- report to developers
  -- action
  -- fixing  by developers
CI is routine task (integrate & report)


Scheduling:
----------
 - On Demand (Build Now)
 - time based (Build Periodically)
 - poll based (Poll SCM)    
Poll SCM : it proceeds for first time and sinks, then 2nd time it will not proceed unless and until new changes happened. (CI job)
Build Periodically: normal job 


Dependences:
===========
 (post build) : if the build get failed, now I need to send a emailÖÖ
	Up stream: before it proceeds to execute a job, it will check for dependences job and execute
	Downstream: once we complete the job at the end it calls another job

Note:- Jenkins Servlet Containers link https://wiki.jenkins.io/display/JENKINS/Tomcat
Environment Variables:-
User variables: specify path including bin directory
System Variables: it is HOME where software installed.

Automated Deployment:
====================

DEV--->BUILD-->TEST-->RELEASE

start your jenkins
install deploy plugin
create a build job in jenkins
add post build actions war/ear containers
keep war file in jenkins home workspace
add a user,role in tomcat-user.xml file
run and validate

Servlet Containers:-  these are containers where Jenkins can install and run, tomcat is very popular.
Glassfish
Tomcat
JBoss
Jetty/Winstone: default servlet container for Jenkins
WebLogic
IBM WebSphere

Tomcat:
------- 
Download and unzip tomcat and place it in one folder
And start tomcat by startup.sh file in bin folder(bash startup.sh)
Start tomcat : ./startup.sh   or  bash startup.sh(in git)
Verufy if tomcat started by going browser ñ http://localhost:8080
Verify if Jenkins is running on tomcat : http://localhost:8080/jenkins

By default Jenkins also run on port 8080. So, now run Jenkins in standalone.
--> If i want to run Jenkins in standalone then go to Jenkins folder and type below
	Java -jar Jenkins. War ñhttpPort=9090. Note: - both will not run on same port 8080.

Note:- Here in Port ìPî is capital 
Note:- to change port in tomcat got to config-->server XML files
Note:- to chang port for jenkins go to D:\jenkins_home-2.73.2\   jave -jar jenkins.war  --httpPort=9090
Note:- to change user,role, and password got to tomcat-users xml files, and give below details

<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<role rolename="admin"/>
<user username="admin" password="admin" roles="admin,manager-gui,manager-script"/>


delivery pipeline:
---------------
create some jobs
and link one job to other
install delivery plugin
start plugin

Email Notification:
-------------------
Configuration: Go to jenkins manage >> config system >> Email Notification(bottom of the page)

SMTP : smtp.gmail.com (for other mial servers go to https://www.arclab.com/en/kb/email/list-of-smtp-and-pop3-servers-mailserver-list.html)
Default e-mail suffix : @gmail.com
Use SMTP Authentication : AlphabetLearn@gmail.com
			: passwd
Use SSL
SMTP Port : 465

        Errors: 1) Login to your gmail account.

		2) Go to https://www.google.com/settings/security/lesssecureapps    and Turn On this feature.

		3) Go to https://accounts.google.com/DisplayUnlockCaptcha      and click Continue. 
                        (OR)

                  Stack Exchange Link :-   https://serverfault.com/questions/635139/how-to-fix-send-mail-authorization-failed-534-5-7-14

JSON and XML format notification plugins, below link
https://wiki.jenkins.io/display/JENKINS/Notification+Plugin --->Notification plugins 

for HTML format notification plugins,below link
https://wiki.jenkins.io/display/JENKINS/Extreme+Notification+Plugin

for advanced email notifications,below link
https://wiki.jenkins.io/display/JENKINS/Email-ext+plugin

Note: install email extension plugin for normal email notifications
Note: by default when build get fails it will send emails and once the same build is success, then
this time it sends success mail also.

jenkins Views:
--------------
In case if you have hundreds of jobs in your jenkins dashboard,the how to view specific catagory jobs.
1.list view
2.nested view (install nested view plugin)

Maven Project:
-------------
maven project setup in jenkins, go to--> global tool configuration
maven path
JDK path

Changing Jenkins Home Dir:
--------------------------
Why: To move from home dir to location where enough space.
project requirement also.
-->C:\Users\vinodh\.jenkins--> All confs, plugins,Logs, secrets...etc/profile
i moved .Jenkins folder files to new folder, And give env variables in system var

Restart:
--------
--> control + c
--> java -jar jenkins.war   (OR)  http:/localhost:8080/restart/
Note:- http:/localhost:8080/systemInfo --> you will get all sys info( "I" Capital)

Command line interface (CLI):- it is very easy, faster, memory management, continues integration
---------------------------
Go to manage jenkins-->Configure global security-->enable security
http://localhost:8080/cli/
-->download cli.jar and test

Users:
------
Create new users
Configure users
Create and manage user roles
Roles strategy plugin ñ download ñ restart Jenkin

CatLight:
---------
>> status notifier for developers
>> catlight will notify your when builds,bugs and tasks need your attention.
>> it is very handy and useful when you have to manage multiple job
	1. Choose things to track
	2. See status in tray
	3. Get notified about the changes
https://catlight.io/	

Blueoccean: Look and feel of jenkins pipelines,jobs,nodes...etc
----------
continuous delivery
-------------------

cd is a step that we will do on the top of ci on which we deploy the application on production like systems(pre-production) and ferform some automation tests.

SonarQube: static code analysis
----------
* open source
* it written in java and support varies languages (like C#,java,ruby,php...etc)
* it supports duplicates, unit tests(fails & success)
* code coverage
* code complexity
* bugs
* week code
	Note:- to install sonarqube need above 11 version of java (in my case i am using JDK14.0.1)
1. https://www.sonarqube.org/downloads/
2. unzip and keep this folder "sonarqube-8.3.0.34182" in your personal drive.
3. open gitbash and navigate to "sonarqube-8.3.0.34182" folder (in my case this is the path) -->> 
    cd a:/VmTutes/Softwares/DevOps/sonarqube-8.3.0.34182/bin/windows-x86-64
4. start startsonar.bat (or) ./startsonar.bat (or) double click on startsonar.bat file in windows GUI
5. sonarqube by default runs on port 9000,  -->> http://localhost:9000
   Note:- we can change the default port-->>Goto-> A:\VmTutes\Softwares\DevOps\sonarqube-8.3.0.34182\conf\sonar.properties
          and uncomment #sonar.web.port=9091

Note:- administration--->> securitys-->> user-->> generate token
6e7787077b51982d5309f50d658be679c29e8b1e


6. Install sonar scanner plugin in jenkins
7. Manage Jenkins --> Configure System -> SonarQube Servers -> Add SonarQube
   Note:- generate sonarqube token to connect to sonarqube from jenkins.
          Go to --> administration-->security-->user-->geneate 
8. Download - https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
9. Manage Jenkins --> Global Tool Configurations -> SonarQube Scanners -> Path of scanner home directory
10.Goto jenkins job -> Add new ->Build ->Add build step ->Execute SonaQube Scanner ->Analysis properties

project properties:- 
# Metadata
   Sonar.ProjectKey=tek
   Sonar.ProjectName=VmTutes
   Sonar.ProjectVersion=1.0
 # Give path to src directory of maven project
   Sonar.sources=target
   Sonar.jacoco.reportPath=target\\coverage-reports\\src

Sonar in EC2
------------
1. java 11 and above is the pre-req for sonarqube server
     > cd /opt
     > oracle java download (type in google) and find link    https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz         under  Java SE Development Kit 17.0.1 downloads ( https://www.oracle.com/java/technologies/downloads/ )     
     > tar -xvzf dk-17_linux-x64_bin.tar.gz
     > sudo su - root
     > vi /etc/profile
     > add  export JAVA_HOME="/opt/jdk-17.0.1"
            export PATH=$PATH:$JAVA_HOME/bin
     > source /etc/profile
     > java -version

2. SonarQube server requries at least 2GB of RAM to run efficiently and 1GB of free RAM for the OS
     note:- Refer:- https://docs.sonarqube.org/latest/requirements/requirements/
     note:- free -h

3. login as root user, switch to /opt dir, download sonarqube server software, and unzip it
    wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.2.0.zip
     note:- unzip /opt/sonarqube-9.2.0

4. as a good security pratice, sonarqube server is not advised to run sonar server as a  root user.
   - adduser sonar
   - give sudo access to sonar user by using "visudo " ALL=(ALL)      NOPASSWD: ALL

5. changing the ownship, group and permissions to /opt/sonarqube-9.2.0
   chown -R sonar:sonar /opt/sonarqube-9.2.0
   chmod -R 775 /opt/sonarqube-9.2.0

6. start sonar
   ->  su - sonar
   ->  cd /opt/sonarqube-9.2.0/bin/linux-x86-64
   ->  ./sonar.sh start
   ->  ./sonar.sh status (confirmation)

Troubleshooting
---------------
sonar server is not starting?
   > check whether java is installed or not by using java -version
   > make sure you changed the ownship and group to /opt/sonarqube-9.2.0 for sonar user.
   > make sure you trying to start sonar service with sonar user.

unable to access sonarqube server URL in browser?
   > make sure the port 9000 is opened in security group in aws ec2 instance.

   >	" WrapperSimpleApp: Encountered an error running main: java.lang.IllegalStateException: SonarQube requires Java 11+ to run
	jvm 1 | java.lang.IllegalStateException: SonarQube requires Java 11+ to run "
	solution
	--------
        Go to--->>  A:\VmTutes\Softwares\DevOps\sonarqube-8.3.0.34182\conf/wrapper.conf
	and change --->> wrapper.java.command=C:\Program Files\Java\jdk-14.0.1\bin\java



jfrog/Nexus (Artifacts)
-----------
jfrog is a place where we store artifacts/packages
Download https://jfrog.com/open-source/
login http://localhost:8081/artifactory/
--> install artifactory plugin from manage plugins
--> manage jenkins ->configure system ->jfrog
     >> uncheck Enable push to bintray option
     >> pass Server ID
     >> URL (Jfrog artifactory url like  http://localhost:8081/artifactory)
     >> UserName |
                 |------>>JFrog Artifactory Credentials
     >> Password |
     >> And click on test connection
     >> Goto job ->configuration-page ->Build Environment section-> select Maven3-Artifactory Integration ->refresh for default
     >> Goto Build section -> select Invoke Artifactory Maven 3 and
                   -> give pom.xml and deploy goal
     >> finally run the job
note:- while running jfrog aritfactory batch file, if it shows "could not reserve enoughf space for object heap" then
       open artifactory batch file in notepad++ and replace "Xms2g" to "Xms1024m" in rem defaults session. 

Jfrog Setup in AWS
-------------------

Pre-requisites:

    An AWS T2.small EC2 instance (Linux)
    Open port 8081 and 8082 in the security group

Installation Steps

    Login to instance as a root user and install Java
     yum install java-1.8* -y 

    Download Artifactory packages onto /opt/
    or Latest version of Artifactory OSS https://jfrog.com/open-source/
    For Older version of Artifactory OSS https://jfrog.bintray.com/artifactory/
    For Latest version of Artifactory Pro https://jfrog.com/artifactory/

    cd /opt 
    wget https://jfrog.bintray.com/artifactory/jfrog-artifactory-oss-6.9.6.zip

    extract artifactory tar.gz file
    unzip jfrog-artifactory-oss-6.9.6.zip

    Go inside to bin directory and start the services
    cd /opt/jfrog-artifactory-oss-6.9.6/bin
    ./artifactory.sh start

    access artifactory from browser
    http://<PUBLIC_IP_Address>:8081 

    Provide credentials
    username: admin
    password: password 

Note:- java installed location in linux:-  /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.amzn2.0.1.x86_64
Note:- maven installed location in linux:- /opt/apache-maven-3.8.1

Job Customization(View):
-----------------------
 default view will be All

Maintanance:
------------

http://localhost:8080/jenkins/exit -->> to shutdown jenkins
http://localhost:8080/jenkins/restart --->> to restart jenkins
http://localhost:8080/jenkins/reload --->> to reload the jenkins configuration

Security:
--------
Configure Global Security
  1. Security Realm :- who to login into jenkins
                       Single sign on(SSO)
                       here jenkins has it's own database
                       Lightweight Directory Access Protocol (LDAP)
  2. Authorization :- Once login into jenkins, what are the permissions required to user..
                      Matrix-based security  

Creating Users,Manage And assign Roles:
---------------------------------------
Prerequisites: 1) install Role-based Authorized plugin
	       2) And enable Role-Based Strategy in Authorization section of Configure Global security option in "manage jenkins"

* Create users by going to manage jenkins >> manage users >> create user
* configure user by going--> "vinodh" right top corner of jenkins page
* assign and manage roles to users
* validate by creating sample jobs


Jenkins Backup Home:
-------------------
 # Jenkins home directory :- where we store all the information about jobs,builds,nodes,logs,plugins,config....etc
 # if we want to take the entire jenkins backup we need to copy the jenkins home dir like "A:\jenkins" and place in other system.
 # (or) we have a plugin called "backup" plugin. 

Jenkins Best Pratices:
---------------------
$ Secure Jenkins always (Configure global security)
$ Not to run many jobs in jenkins master server. only run on critical situations like backups...
$ Backup jenkins home directory regurarly.
$ Setup your jenkins on partition which has more free disk space.
$ archive unused jobs
$ not to schedule all the jobs at the same time(jenkins performance issue) 



pipeline
=========
Def:- In jenkins, a pipeline is a group of events/jobs which are interlinked with one another in a sequence.
      |---job1---||---job2---||---job3---||---job4---|
   
     - and every job in pipeline has some dependencys on one/more jobs


We can create jenkins pipelines in two ways
1. using Build and Delivery Pipelines
2. using scripted and Declaritive pipelines

Note:- Mostly we use delclarative in realtime

# Scripted 
     - The scripted pipeline is a traditional way of writing the Jenkins pipeline as code.
     - Scripted pipeline is written in Jenkinsfile.
 
node {
    stage('Development') {
        echo " developing the project"
    }
    stage('Build') {
        echo "performing maven project"
    }
    stage('Testing') {
        echo "Testing the project"
    }
    stage('Release') {
        echo "Releasing the project"
    }

}


# Declartive
	- rather then following the jenkins standards, we can create our own workflow/process.

	>> Step:- Specify what step we want to do first,2nd, 3rd....etc
            eg:- after build i do not want to connet to SCM
                 after completing of one task i want to connect to scm
	>> Stage:- collection of steps
                   and we can keep group of steps in one stage and specify where you want to run
	>> Groovy Script:- Advanced script which is only designed for jenkins.

       - jenkinsfile:- jenkins file is a text file that contains definition of a jenkins pipeline and it will checked into the SCM.
 

pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                echo "Compiling the Code.........."
                bat "mvn compile"
            }
        }
        stage('Testing') {
            steps {
                echo "Testing the Code.........."
                bat "mvn test"
            }
        }
        stage('Packaging') {
            steps {
                echo "Packaging the Code.........."
                bat "mvn package" 
            }
        }
       stage('Install') {
            steps {
                echo "Installing the Code.........."
                bat "mvn install" 
            }
        }
    }
}


Eg2:-
-----
pipeline {
    agent  {
       node  {
          label "mygroup"
          customworkspace "A:\sam/PIPE"
       }
   }
   triggers {
      cron('* * * * *')
   }
   stages {
       stage('stage1') {
           steps {
              echo 'hello welcome to groovy script stage I'
           }
       }
       stage('stage2') {
           steps {
              echo 'hello welcome to groovy script stage II'
           }
       }
       
   }
   post {
           always {
               echo 'End of groovy script pipeline'
           }
       }
}




Note:- Changing boot configuration.
By default, your Jenkins runs at https://localhost:8080/. This can be changed by editing jenkins.xml , which is located in your installation directory. This file is also the place to change other boot configuration parameters, such as JVM options, HTTPS setup, etc.


                           				 ====THE END====       
			VmTutes, +91-7204143230(WhatsApp/Call), Email:- Vinodh-Machireddy@VmTutes.com












