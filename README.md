# CI-CD Pipelines

![image](https://user-images.githubusercontent.com/110126036/187884312-e9ba242b-721e-4b16-8c06-5493d3c8b0ba.png)

## CI (Continuous Integration)

Continuous integration is all about merging a developers code into the master branch of the code. This is done automatically with the use of a web hook.

A webhook is a lightweight API which shares data in one direction and is triggered by events (such as a push from the localhost).

The webhook is always listening and waiting for an event to happen which allows for it to be continuous.

After the code is integrated it will give the merged code to an automation server such as Jenkins.

## CD (Continuous Delivery/Continuous Deployment)

In CD automatic testing occurs it is then passed onto a cloud service provider where it can be deployed (if the tests pass).

The CD part can either mean continuous delivery or continuous deployment. The key difference being that with continuous delivery you will manually have to deploy the program, whereas continuous deployment will automatically do this.

One reason for opting for continuous delivery over deployment could be that perhaps the team wish to conduct more tests on the code (beyond the unit tests).

Useful links

WEBHOOK setup:

https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project

GIT MERGE ONTO MAIN setup:

https://andrewtarry.com/posts/jenkins_git_merges/

CONTINUOUS DEPLOYMENT (rsync link)

https://github.com/khanmaster/jenkins_cicd_aws

```
rm -rf eng84_cicd_jenkins*
git clone -b main https://github.com/Olejekglejek/CI_CD_Jenkins.git

rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@deploy_public_ip:/home/ubuntu/app
rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@deploy_public_ip:/home/ubuntu/app

ssh -A -o "StrictHostKeyChecking=no" ubuntu@deploy_public_ip <<EOF

    # 'kill' all running instances of node.js
    killall npm

    # run provisions file for dependencies
    cd /home/ubuntu/app/environment/app
    chmod +x provision.sh
    ./provision.sh

    # Install npm for remaining dependencies
    cd /home/ubuntu/app/app
    sudo npm install
    node seeds/seed.js

    # Run the app
    node app.js &

EOF
```

Steps: 

- Create EC2
- Create Security group
- Allow Jenkins IP to ssh in aswell as any tools required
- Create a 3rd job in Jenkins: get the code from main branch and copy (SCP) to the ec2 instance. Run the script to install dependencies with any other required dependencies
- The 3rd job must only be triggered if 2nd job was successful
- 4th job launch the app if 3rd job 
- pm2 kill all create a 5th job to create DB))HOST = db-ip
- npm start
