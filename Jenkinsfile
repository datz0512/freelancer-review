
namespace = "production"
serviceName = "freeln-review"
service = "Freelaner Reviews"

pipeline {
  agent {
    label "Jenkins-Agent"
  }

  tools {
    nodejs "NodeJS"
    dockerTool "Docker"
  }

  environment {
    DOCKER_CREDENTIALS = credentials("dockerhub")
    IMAGE_NAME = "datz0512" + "/" + "freelancer-review"
    IMAGE_TAG = "stable-${BUILD_NUMBER}"
  }

  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()
      }
    }

    stage("Prepare Environment") {
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/datz0512/freelancer-review'
        sh 'npm install'
      }
    }

    stage("Lint Check") {
      steps {
        sh 'npm run lint:check'
      }
    }

    stage("Code Format Check") {
      steps {
        sh 'npm run prettier:check'
      }
    }

    stage("Unit Test") {
      steps {
        sh 'npm run test'
      }
    }

    stage("Build and Push") {
      steps {
        sh 'docker login -u $DOCKERHUB_CREDENTIAL_USR --password $DOCKERHUB_CREDENTIALS_PSW'
        sh "docker build -t $IMAGE_NAME ."
        sh "docker tag $IMAGE_NAME $IMAGE_NAME:$IMAGE_TAG"
        sh "docker tag $IMAGE_NAME $IMAGE_NAME:stable"
        sh "docker push $IMAGE_NAME:$IMAGE_TAG"
        sh "docker push $IMAGE_NAME:stable"
      }
    }

    stage("Clean Artifacts") {
      steps {
        sh "docker rmi $IMAGE_NAME $IMAGE_NAME:$IMAGE_TAG"
        sh "docker rmi $IMAGE_NAME $IMAGE_NAME:stable"
      }
    }
  }
}