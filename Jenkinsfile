pipeline{
  agent any
  environment {
    IMAGE_NAME = 'parth2k3/test-flask'
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/commodorebob/test-flask'
      } 
    }
    stage('Build Docker image'){
      steps{
        bat "docker build -t %IMAGE_NAME%:latest ."
      }
    }
     stage('Push to Dockerhub'){
      steps{
        withCredentials([usernamePassword(credentialsId: '12eeebe0-edbb-461d-b452-22ba234f5af3', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        bat """
        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
        docker push %IMAGE_NAME%:latest
        docker logout
        """
        }
      }
    } 
  }
}
