pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    IMAGE_NAME = 'test-flask'
    REPO_NAME = 'test'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/commodorebob/test-flask'
      }
    }

    stage('Tag the image') {
      steps {
        script {
          env.IMAGE_TAG = 'latest'
        }
      }
    }

    stage('Login to ECR') {
      steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_creds']]) {
  powershell '''
    $env:AWS_ACCESS_KEY_ID = "${AWS_ACCESS_KEY_ID}"
    $env:AWS_SECRET_ACCESS_KEY = "${AWS_SECRET_ACCESS_KEY}"

    $ecrLogin = aws ecr get-login-password --region $env:AWS_REGION
    docker login --username AWS --password $ecrLogin https://004234227389.dkr.ecr.us-east-2.amazonaws.com
  '''
}
      }
    }

    stage('Build Docker Image') {
      steps {
        powershell '''
          docker build -t ${env:IMAGE_NAME}:${env:IMAGE_TAG} .
          docker tag $env:IMAGE_NAME:$env:IMAGE_TAG 004234227389.dkr.ecr.us-east-2.amazonaws.com/test:latest
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        powershell '''
          docker push 004234227389.dkr.ecr.us-east-2.amazonaws.com/test:latest
        '''
      }
    }
  }
}
