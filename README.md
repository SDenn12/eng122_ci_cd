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

## Jenkins

Jenkins is an open source automation server. You can create jobs which will execute a code or process. These jobs can be triggered from other events such as the completion of a different job or from a webhook that can be set up on platforms such as github (version control service). 

## Webhooks

When we push from our localhost on the developers branch the code is sent to github which acts as a webhook. This effectively means it is always listening for changes on github and starts the automation process if any change is detected. The webhook can be configured in the repository and the payload URL should be the jenkins URL. On Jenkins you can set a trigger on a job for whenever the webhook is activated.
