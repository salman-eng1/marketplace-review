namespace = "production"
serviceName = "marketplace-review"
service = "Marketplace Reviews"


pipeline {
  agent {
    label 'Jenkins-Agent'
  }

  tools {
    nodejs "NodeJS"
    dockerTool "Docker"
  }

  environment {
    DOCKER_CREDENTIALS = credentials("dockerhub")
    IMAGE_NAME = "eng9" + "/" + "marketplace-review"
    IMAGE_TAG = "stable-${BUILD_NUMBER}"
  }

  stages {
    stage("Cleanup Workspace"){
        steps{
            cleanWs()
        }
    }
    stage("Prepare Environemnt")
    steps{
      git branch: 'master', credentialsId: 'github', url: 'https://github.com/salman-eng1/marketplace-review.git'
      sh 'npm install'
    }
  }
}