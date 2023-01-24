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

## Example
Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage. Go ahe

                                                      
  ![Jenkinsfile-Build stage  new](https://user-images.githubusercontent.com/65022146/214299598-36be0b65-df7c-49c9-98b9-8238382c86cc.png)
  
 
 - If this job is triggered in Jenkins(and in Blue Ocean) and the Build is successful, you will see an output thats looks like the screenshot below:


![branch echo-building stage](https://user-images.githubusercontent.com/65022146/214286301-38e8a344-bf82-4449-89fb-c35aa3b62787.png)


- At this point, another branch called 'feature/jenkinspipeline-stages'. This is because when you are working on a project, you do not want to be commiting directly to the main branch. it is always good to work on branch and only merge with the main branch when the code is good enough and have passedm the necessary test.

- Next, add another stage called 'Test', push the changes to github and try to trigger the build in Jenkins. Here, we are also going to add a quick task which include: Packages, Deploy ad Clean-up Stages--both Initial clean up of Jenkins directory and the eventual deletion of workspace and folders after jobs must have been built.
![Jenkinsfile-test-cleanup stage 1](https://user-images.githubusercontent.com/65022146/214308942-17771510-f5bc-4c56-bb50-e6a068084ec6.png)
     ![Jenkinsfile-test-cleanup stage 2](https://user-images.githubusercontent.com/65022146/214308946-dbbfd5ed-68cc-4b37-8e7a-db0257824ea9.png)
     
-  If the build is successful, you will see a screenshot like this:
![delete workspace successful](https://user-images.githubusercontent.com/65022146/214311188-d3be6a5c-0a27-4147-9873-8b4af62c7c04.png)



## RUNNING ANSIBLE PLAYBOOK FROM JENKINS
Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

- Installing Ansible on Jenkins
- Installing Ansible plugin in Jenkins UI
- Creating Jenkinsfile from scratch. (This time you have to delete all you currently have in Jenkinsfile and start all over to get Ansible to run successfully)

- The new Jenkinsfile will look like the screenshot below:

![Jenkinsfile for ansible plabk1](https://user-images.githubusercontent.com/65022146/214329229-fade2a13-0170-4c35-a110-b6986b95c0a8.png)
![Jenkinsfile for ansible plabk2](https://user-images.githubusercontent.com/65022146/214329237-a820abb4-c65e-4b79-974c-fe0f7b20b06d.png)



- Note that for Ansible to install MYSQL, there are some dependecies that needs to be installed for ansible to run properly

`
python3 -m pip install --upgrade setuptools
python3 -m pip install --upgrade pip
python3 -m pip install PyMySQL
python3 -m pip install mysql-connector-python
`

- For Postgresql Database, install

  `ansible-galaxy collection install community.postgresql`
 
 #### Here two more instances were added namely: Nginx(RHEL) and Database(Ubuntu)
 
#### Befoe running the ansible playbook, do not forget that ansible need to ssh into the instances. And because, ansible is not directly running the playbook, the private key of the instances has to be passed over to Jenkins by going to:

- Jenkins------>Manage Jenkins---->Manage Credentials

#### Also add ansible to Jenkins by navigating to:

 - Jenkins------>Manage Jenkins---->Global Tool Configuration

#### Note also that you can generate the ansible-playbook string by navigating to:

 - Jenkins--->Ansible-config-mgt(Repo name)---->Pipeline Syntax
 
  Now let us run an ansible-playbook in the Jenkinsfile, Here you have to tweak the Playbook/site.yml by uncommenting the Nginx and Database roles and also in Mysql/defaults/main.yml, uncomment the Creating a database, tooling and the user, Webaccess
        
