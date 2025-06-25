pipeline {
  agent any

  environment {
    IMAGE_NAME = "shobana29/flask-k8s-app"
    TAG = "${BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        echo "Code already checked out via SCM"
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:${TAG}")
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhub-creds', url: '') {
          script {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}

