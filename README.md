# Deployment 3 Documentation
## Purpose:
The purpose of Deployment 3 was to set up Jenkins to build/test/deploy applications automatically to AWS Elastic Beanstalk using GitHub Webhooks. Webhooks will be triggered whenever a specific event occurs on GitHub, allowing us to automate the deployment process whenever there are changes within the GitHub repository.

## Steps:
Before we start, I also have to download Jenkins onto my AWS EC2 instance. To see the steps to install the Jenkins server (Steps 1 and 2) click [here](https://github.com/auzhangLABS/Installing-Jenkins). 
1. After confirming that we have Jenkins on my EC2 instance, We would have to install a couple of packages
   -`sudo apt install python3-venv`
     -Downloading Python Virtual Environment
   -`sudo apt install python3-pip`
     -Installing python3 pip (line package manager and installer)
   -`sudo apt install unzip`
     -Downloading unzip software package
2. Created a multibranch pipeline (allows a set of pipelines to detect branches). For Jenkins to find our Github file, I selected the option called branch sources and chose Github, and entered my Github link and credentials.
3. Once, Jenkins was able to build and test my application successfully, then I installed my AWS CLI
   -Click IAM - Click users under IAM resources - Click my username - Click Security Credentials - Click Create access key - Select Command Line Interface (CLI) - Click Confirmation - Click Next - Enter AWS_CLI into the Description tag values. Once I generated, I stored my access key and secret access key
  <br> -Create a password for Jenkins using: 
     `sudo passwd jenkins` 
  <br> -Log into Jenkins user using:
     `sudo su - jenkins -s /bin/bash`
  <br>-download ebcli (Elastic Beastalk CLI):
     `pip install --upgrade --user awsebcli`
  <br> -changes the PATH environment variable for the current system:
     `export PATH=$PATH:$HOME/.local/bin`
  <br> -check if eb works and exists
     `eb --version`
  <br> -set my AWS credentials
     `aws configure`
4. Created an EB environment
   -within the console, cd into the workspace as the logged-in Jenkins user.
    <br> `cd workspace`
    <br> `cd Deployment_3`
   - Configure a local repository and create an environment/deploy in Elastic Beanstalk.
     <br> `eb init` with the default option and selecting n for code commit and n for ssh.
     <br> `eb create` with the default option and selecting n for spot fleet.
   - once EB finished, it would present the application URL within the console.
5. Added a Deploy stage within Jenkins:
   <br> `stage ('Deploy') { steps { sh '/var/lib/jenkins/.local/bin/eb deploy' } }`
   <br> -after making this change I noticed that created a new stage called Deploy in the pipeline. Additionally, it started the environment and deployed the new version to the instance.
6. Adding webhook to trigger Jenkins when a push event is added to my GitHub.
   <br> -for the Payload URL, I add http://yourIP:8080/github-webhook/
   <br> -from here, I was able to see recent deliveries, and each time I change something in my repository it would be shown here.
7. To test that this works. I edit my template/home.html files. After committing that change, I noticed that I was able to the changes in the recent deliveries. Over at Jenkins, I can see that it automates the process of build, testing, and deploying. Over at AWS Elastic Beanstalk, I noticed that my environment events are also updating to the new version. On the domain, I can see the changes I made. 

## System Design Diagram:
To view the System Design Diagram, click [here!](https://github.com/auzhangLABS/c4_deployment3/blob/main/diagram.png)

## Issues/ Troubleshooting:
The main issue I encountered was with the pip package command. When I signed into Jenkins user and tried to run the pip command it would give me an error. I diagnose this problem by noticing that python and python3 are two different versions. Once, I was able to figure that out I ran `sudo apt install python3-pip` instead of `sudo apt install python-pip`

## Optimization:
I would consider creating an bash script that automates the installation of Jenkins on your instance for optimization
