pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "balav8/node:${env.GIT_COMMIT}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run tests') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build and push Docker image') {
      steps {
        script {
          docker.build(env.DOCKER_IMAGE)
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials-id') {
            docker.push(env.DOCKER_IMAGE)
          }
        }
      }
    }
  }
}
