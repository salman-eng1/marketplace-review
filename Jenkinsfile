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
    IMAGE_NAME = "eng9/marketplace-review"
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
        script {
          // Inject GitHub credentials into the environment
          withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
            // Replace the contents of the existing .npmrc file
            sh """
              echo "//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}" > .npmrc
              echo "@salman-eng1:registry=https://npm.pkg.github.com/salman-eng1" >> .npmrc
            """
            // Checkout the Git repository
            git branch: 'master', credentialsId: 'github', url: 'https://github.com/salman-eng1/marketplace-review.git'
          }
          // Install npm dependencies
          sh 'npm install'
        }
      }
    }
  }
}
