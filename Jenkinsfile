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

            // Checkout the Git repository
            git branch: 'master', credentialsId: 'github', url: 'https://github.com/salman-eng1/marketplace-review.git'
                        // Replace the contents of the existing .npmrc file
                    sh '''
                        echo "Creating .npmrc file..."
                        echo "//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}" > .npmrc
                        echo "@salman-eng1:registry=https://npm.pkg.github.com/salman-eng1" >> .npmrc
                        echo "File content:"
                        cat .npmrc
                    '''
          }
          // Install npm dependencies
          sh 'npm install'
          sh 'cat .npmrc'
          sh 'rm .npmrc'
        }
      }
    }
  }
}
