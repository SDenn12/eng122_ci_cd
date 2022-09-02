## Automating the CI Pipeline

We want to automate the integration and deployment of the nodeapp from the development environment all the way to the production environment (on AWS). This will be NOT including infrastructure as code services such as Ansible and Terraform and so we will create the EC2 instances manually. 

### Establishing the connection between GitHub (Version Control Service) and the LocalHost (Development Environment)

1. Open GitBash and naviagte to .ssh folder
2. Create a key using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` using your own GitHub email address
3. Add to the SSH agent `eval "$(ssh-agent -s)"` and then `ssh-add ~/.ssh/KEYNAMEprivate`
4. Copy the public key to GitHub 

### Establishing the connection between Github and Jenkins (Automation Server)

1. Create another SSH key and add it to the agent using the above methods.
2. Copy the public key into your Github repository in the settings, allowing write access. 

### Setting up a Webhook

1. Go to settings on your GitHub repository
2. Click webhooks and add a webhook with the following settings

![image](https://user-images.githubusercontent.com/110126036/188120513-50151f56-6b5b-43c7-a1ef-47ee224744be.png)

3. Ensure that in the payload URL http://JENKINS_IP/github-webhook/

### Creating Jobs

1. Create a job for retrieving the data from the dev branch and running tests on the code

![image](https://user-images.githubusercontent.com/110126036/188121286-0774134c-05ae-4a5e-b080-e32fc4f2a316.png)
![image](https://user-images.githubusercontent.com/110126036/188121420-69697487-718c-4f07-8571-1f4e8d1b8671.png)
![image](https://user-images.githubusercontent.com/110126036/188121480-60324bac-7643-494d-ab84-c58c8767101e.png)
![image](https://user-images.githubusercontent.com/110126036/188121525-86bb587c-e731-4d0f-bf29-06731886ae4d.png)
![image](https://user-images.githubusercontent.com/110126036/188121573-7ebee24d-b02a-4bf7-85e4-15e79783c3ca.png)


2. Create a job for merging the dev branch into the main branch if the tests are successful

![image](https://user-images.githubusercontent.com/110126036/188121734-3e290a7b-ed2a-4eef-b884-d6dc7741b72e.png)
![image](https://user-images.githubusercontent.com/110126036/188121793-53239ca5-47a2-4c6c-afe7-67338cc9498a.png)

## Automating the CD pipeline

Steps: 

- Create EC2
- Create Security group
- Allow Jenkins IP to ssh in aswell as any tools required
- Create a 3rd job in Jenkins: get the code from main branch and copy (SCP) to the ec2 instance. Run the script to install dependencies with any other required dependencies
- The 3rd job must only be triggered if 2nd job was successful
- 4th job launch the app if 3rd job 
- pm2 kill all create a 5th job to create DB))HOST = db-ip
- npm start


Useful links

WEBHOOK setup:

https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project

GIT MERGE ONTO MAIN setup:

https://andrewtarry.com/posts/jenkins_git_merges/

CONTINUOUS DEPLOYMENT (rsync link)

https://github.com/khanmaster/jenkins_cicd_aws
