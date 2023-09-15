# Deployment 3 Docmentation
## Purpose:
The purpose of deployment 3 was to set up Jenkins to build/test/deploy applications to automatically deploy AWS Elastic Beanstalk. We were able to do this using Github Webhooks. Webhooks will be triggered whenever a specific event occurs on GitHub. With this, we could automate the deployment process whenever there were changes within the GitHub repository.

## Steps:
Before we start, I also have to download Jenkins onto my AWS EC2 instance. To see the steps to install the Jenkins server (Steps 1 and 2) click [here](https://github.com/auzhangLABS/Installing-Jenkins). 
1. After confirming that we have Jenkins on my EC2 instance, We would have to install a couple of packages
   -'sudo apt install python3-venv'
     -Downloading Python Virtual Environment
   -'sudo apt install python3-pip'
     -Installing python3 pip (line package manager and installer)
   -'sudo apt install unzip'
     -Downloading unzip software package
2. Created a multibranch pipeline (allows a set of pipelines to detect branches). For Jenkins to find our Github file I selected the option called branch sources and chose Github and entered my Github link and credentials.
3. Once, Jenkins was able to build and test my application successfully, then I installed my AWS CLI
   -Click IAM - Click users under IAM resources - Click my username - Click Security Credentials - Click Create access key - Select Command Line Interface (CLI) - Click Confirmation - Click Next - Enter AWS_CLI into the Description tag values. Once I generated, I stored my access key and secret access key
  -Create a password for Jenkins using:
     'sudo passwd jenkins'
  -Log into Jenkins user using:
     'sudo su - jenkins -s /bin/bash'
   -download ebcli (Elastic Beastalk CLI):
     'pip install --upgrade --user awsebcli
   -changes the PATH environment variable for the current system:
     'export PATH=$PATH:$HOME/.local/bin'
   -check if eb works and exists
     'eb --version'
   -set my AWS credentials
     'aws configure'
4. Created an EB environment
   -within the console, cd into the workspace as the logged-in Jenkins user.
     'cd workspace'
     'cd Deployment_3'
   - Configure a local repository and create an environment/deploy in Elastic Beanstalk.
     'eb init' with the default option and selecting n for code commit and n for ssh.
     'eb create' with the default option and selecting n for spot fleet.
   - once EB finished, it would present the application URL within the console.
5. Added a Deploy stage within Jenkins:
   'stage ('Deploy') { steps { sh '/var/lib/jenkins/.local/bin/eb deploy' } }'
   -after making this change I noticed that created a new stage called Deploy in the pipeline. Additionally, it started the environment and deployed the new version to the instance.
6. Adding webhook to trigger Jenkins when a push event is added to my GitHub.
   -for the Payload URL, I add http://yourIP:8080/github-webhook/
   -from here, I was able to see recent deliveries, and each time I change something in my repository it would be shown here.
7. To test that this works. I edit my template/home.html files. After committing that change, I noticed that I was able to the changes in the recent deliveries. Over at Jenkins, I can see that it automates the process of build, testing, and deploying. Over at AWS Elastic Beanstalk, I noticed that my environment events are also updating to the new version. On the domain, I can see the changes I made. 
