pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1' // ✅ match the ECR region
    IMAGE_NAME = 'test-flask'
    REPO_NAME = 'test'
    IMAGE_tag = 'latest'
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
    withCredentials([[
      $class: 'AmazonWebServicesCredentialsBinding',
      credentialsId: 'aws-creds'
    ]]) {
      powershell '''
        $Env:AWS_ACCESS_KEY_ID = "$env:AWS_ACCESS_KEY_ID"
        $Env:AWS_SECRET_ACCESS_KEY = "$env:AWS_SECRET_ACCESS_KEY"
        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin https://004234227389.dkr.ecr.us-east-1.amazonaws.com
      '''
    }
  }
}

    stage('Build Docker Image') {
      steps {
        powershell '''
          docker build -t ${env.IMAGE_NAME}:${env.IMAGE_TAG} .
          docker tag ${env0.IMAGE_NAME}:${env.IMAGE_TAG} 004234227389.dkr.ecr.us-east-1.amazonaws.com/test:latest
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        powershell '''
          docker push 004234227389.dkr.ecr.us-east-1.amazonaws.com/test:latest
        '''
      }
    }
  }
}
