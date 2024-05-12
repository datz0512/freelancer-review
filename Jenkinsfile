
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
  }
}