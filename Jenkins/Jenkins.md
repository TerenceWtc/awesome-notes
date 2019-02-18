# Jenkins Installation

## Preparation
- OS (CentOS 7)
- Java environment (JAVA_HOME)
- Download Jenkins rpm file (`jenkins-2.89.3-1.1.noarch.rpm`)

## Installation
### 1. Import public key
> rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

### 2. Install
> rpm -ivh jenkins-2.89.3-1.1.noarch.rpm

## Configuration
After installing Jenkins, a config file will be created in `etc/sysconfig` called `jenkins`. This file contains all the configurations
  > $ vim /etc/sysconfig/jenkins

you may find many variable:
  - `JENKINS_HOME` stands for the path where your source code pulled and built
  - `JENKINS_JAVA_CMD` stands for the same to `JAVA_HOME`
  - `JENKINS_USER` means the one who will startup Jenkins, should have the access permission to both `JENKINS_HOME` and `/var/log/jenkins`
  - `JENKINS_PORT` stands for the running port

## Start
- Startup command
  > service jenkins start
- Check status
  > systemctl status jenkins.service

  *if shows `Active: active (exited)`*
  *may be the `JENKINS_USER` have no permission to access `JENKINS_HOME` or `/var/log/jenkins`*

## Initialization
### 1. Visit Jenkins
Use `JENKINS_PORT` to visit Jenkins dashboard

### 2. Initial password
Once you're the first time to visit Jenkins, the dashboard will display where your initial password in

### 3. Install default plugins
Following the steps, it will guide you to install default plugins

### 4. Finish
Finally, it will guide you to create an admin privilege account

---
# New a Jenkins Project
After installation, a dashboard should be displayed.  
In the left side, there's a menu.  
![LeftDashboard](https://github.com/TerenceWtc/documents/blob/master/Jenkins/LeftDashboard.png)

- Click `New Item`  
  Enter a item name and choose the `FreeStyle project`.
- Fill up the project
  - General  
  Some common things like description.
  - Source Code Management  
  We use `Git` to manage the version of source code.
  Copy your `Repository URL` and create a `Credentials` with you repository account, make sure Git is ready on your server.
  You can also specify the branch and the default is `*/master`.
  - Build  
  Contains steps of build, publish, restart and so on.  
  You can choose shell script or windows batch command to do that:
    > $mvn clean package  
    or  
    > $npm run build: prod
    or any other scripts.  
  
    If you have a server cluster, and seperate Jenkins from others, a Jenkins plugin `Publish Over SSH` is helpful for you.

- Save and have the first try  
  After finishing above, save it and will return to the dashboard.
  Choose your first Jenkins project and click `Build now`, Jenkins will start to pull the source code, build, and publish.
  Congratulations if you see the success message. If failed or unstable, check the configure of your Jenkins project.

---
  



TO DO:  
- [x] new a Jenkins item  
- [ ] ...