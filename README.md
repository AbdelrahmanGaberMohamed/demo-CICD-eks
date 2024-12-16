![image](https://github.com/user-attachments/assets/35da3bae-a4f4-47dc-b414-4cf30b296f08)

This Repo is a demo project to create a CICD pipeline with Jenkins for EKS deployments.
Project prerequisites:
  - Dockerhub repo created.
  - AWS EKS cluster created and a deployment is running with a dummy image.

Clone this repo to your machine:
  - git clone https://github.com/AbdelrahmanGaberMohamed/demo-CICD-eks
  - create a new repo in your GitHub and push the code to it
  - Note: If you create the repo as private you will need to pass credentials for Jenkins to authenticate with GitHub

Using Jenkins credential manager create the following credentials:
  - DockerHub, type: username and password [Used  to authenticate with dockerhub to store successful builds of docker images].
  - amazon_access_key, type: secret text [Used to authenticate with AWS]
  - amazon_secret_access_key, type: secret text [Used to authenticate with AWS]

Create a new pipeline and set the trigger to the GitHub hook (Check this tutorial on how to create GitHub hooks https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project).

The first Run must be triggered manually.

After a new commit is added to the repo the pipeline will be triggered via GitHub hooks and a new build will start
Stages:
  - Setup: Set up a Python virtual environment and install the required dependencies.
  - Test: Use pytest to run the included unit tests.
  - Login to docker hub: Authenticate with docker hub using the credentials we created in Jenkins credentials manager.
  - Build Docker Image: On successful test build a Docker image of the code using the included Dockerfile.
  - Push Image To Dockerhub: Push the created image to Dockerhub.
  - Deploy to EKS: Update your deployment with the new image we just created.


