## EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

Project 14 is a typical example of an End-to-End Implementtion of CI(Continous Integration) and CD(Continuous Delivery and Deployment) process. This project made use of Jenkins, Ansible, Artifactory, Sonarqube and PHP Application.

- Why do we need SonarQube?
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.

- Why do we need Artifactory?
Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.

### SET UP
This project is partly a continuation of your Ansible work which we have seen from Project 11, so simply add and subtract based on the new setup in this project. It will require a lot of servers to simulate all the different environments from dev/ci all the way to production.

To get started, we will focus on these environments initially.
- Ci
- Dev
- Pentest

### Update the Ansible-Config-mgt repository by:
- Creating a folder called Deploy which contains the Ansible.cfg and Jenkinsfile
- Updating the inventory file by adding these files to it: ci.yml, pentest, Pre-prod, Prod
- Adding two more ROLES namely Artifactory and Sonarqube Roles

-  Next thing you have to do is to lauch and connect our Jenkins instance. And then install Jenkins but before that, you have to install git in order to clone the Repository. 

-Having installed git and cloned down the Ansible-config-mgt repository, you install Jenkins by:
    
    `sudo wget -O /etc/yum.repos.d/jenkins.repo \https://pkg.jenkins.io/redhat-stable/jenkins.repo`
    
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    
    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    `yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`

- Install Java

`sudo yum install java-11-openjdk-devel -y`

- Open a Batch Profile

 vi .bash_profile

` 
  export JAVA_HOME=$(dirname= $(dirname $(readlink $(readlink$(which javac)))))
  export PATH=$PATH:$JAVA_HOME/bin 
  export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
`

- Reload bash profile

`source ~/.bash_profile`
 
 - Having installed Java and Epel release, next is to install Jenkins:

   `sudo yum install jenkins`
   
 - This is followd by: Starting, Enabling, View Jenkins status and also sudo systemctl daemon-reload

#### Set up Jenkins by creating the Admin user with the Password:
- Install Blue Ocean pluggin
- Create a new pipeline
- Create an Access Token

#### Create a Jenkinsfile and run Jenkinsfile configuration with Jenkins

- Jenkinsfile contains every step or jobs/actions that we want to execute. It is built in a code. 
- Every job in Jenkinsfile starts with a Pipeline followed by Agent. In the cousre of this project, we use 'Any Agent', followed by the Stages ad steps.
   
