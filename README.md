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
 
  Now let us run an ansible-playbook against ci environment in the Jenkinsfile, Here you have to tweak the Playbook/site.yml by uncommenting the Nginx and Database roles and also in Mysql/defaults/main.yml, uncomment the Creating a database, tooling and the user, Webaccess
  
   
-  If the build is successful, you will see a screenshot like this:
        
    ![ansi inv ci succesful](https://user-images.githubusercontent.com/65022146/214336530-56c0905e-ad4a-4c8e-bf48-a9255b23714f.png)
    
    
    #### Parameterization
- The ansible-playbook above was ran under the ci env. However, Ansible can run in a different environment such as in Dev by using build with parameter
    
       ![Build with parameter](https://user-images.githubusercontent.com/65022146/214342717-d944acef-28b8-4919-8ca2-7b47f0700cb4.png)


- Now let's run an ansible playbook with Dev parameter. This will look like the image below if successful:

    ![ansible wt dev parameter successful](https://user-images.githubusercontent.com/65022146/214344183-52b8a197-912f-48d1-905d-725235fbcc17.png)
    
    
    
    ## CI/CD PIPELINE FOR TODO APPLICATION
    
    - Here you have to add PHP-Todo folder to your workspace.
    
 #### Phase 1 – Prepare Jenkins by:
   
   1. Forking and cloning down the PHP-Todo repository
   
   2. On you Jenkins server, install PHP, its dependencies and Composer tool as follows:
     
  
        `yum module reset php -y`
        
        `yum module enable php:remi-7.4 -y`
        
        `yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json`
        
        `systemctl start php-fpm`
        
        `systemctl enable php-fpm`
        
        
 - Install Composer
    
    ` curl -sS https://getcomposer.org/installer | php
     sudo mv composer.phar /usr/bin/composer`
        
        
    - Verify Composer is installed or not   
         
         `composer --version`
         
         
#### Install Jenkins plugins
    
    - Plot plugin
    - Artifactory plugin

#### We will use plot plugin to display tests reports, and code coverage information.
#### The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server. 

- Launch artifactory instance
- Update the Inventory/ci with the artifactory private IP address
- Tweak the Playbook/site.yml by uncommenting the Artifactory role/assignment

#### Next, run the ansible playbook using the ci parameter to install the artifactory.

- If the playbook is successful, you will see an image like the screenshot below:

     ![artifactory ansi-playbook-ci succesful](https://user-images.githubusercontent.com/65022146/214430855-a94febce-f7c7-4a08-b5c1-5c946b8b98d2.png)
     
 
 
 
 - In Jenkins UI configure Artifactory  
 - Configure the server ID, URL and Credentials, run Test Connection.


#### Phase 2 – Integrate Artifactory repository with Jenkins

- Create a dummy Jenkinsfile in the repository
- Using Blue Ocean, create a multibranch Jenkins pipeline

- On the database server, create database and user: Here, navigate to Roles/Mysql/defaults/main.yml to tweak the configuration for mysql installation of Database and the user.

- Also tweak the .env.sample
- Install mysql on Jenkin server
- Set the bind address 

   ![ansi mysql successful 1](https://user-images.githubusercontent.com/65022146/214438263-a15123f1-cc0f-4a32-94b1-87943681cf95.png)
   ![ansi mysql successful 2](https://user-images.githubusercontent.com/65022146/214438266-e99bbeea-944e-4439-ac37-2ccfb3eaa35a.png)
   
   
   #### Confirm the installation by logging into the database:
   
     ![connected to mysql via phptodo](https://user-images.githubusercontent.com/65022146/214441924-dc166023-034f-4bdd-910a-5e06603b8bbb.png)
     
     
     
     
    #### Update Jenkinsfile with proper pipeline configuration to include the following stages:
    
    - Initial cleanup
    - Checkout SCM
    - Prepare Dependencies
    - Execute Unit Tests


- If the playbook is successful, you will see an image that looks like the screenshot below:

     ![Execute unit test successful](https://user-images.githubusercontent.com/65022146/214448400-297e6a51-6fa7-4303-b38a-5ab16ef4c381.png)


#### Also add the following Stages to the Jenkinsfile:

- Add the code analysis step
- Plot Code Coverage Report

- ![Plot code job successful](https://user-images.githubusercontent.com/65022146/214450921-d23eef8a-7b9a-46ba-a676-9ce86303c8f1.png)
If the playbook is successful, you will see an image that looks like the screenshot below:


