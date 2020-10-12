pipeline {

  environment {
    registry = "192.168.1.126:5000/justme/myweb"
    dockerImage = ""
  }

  agent any
    
  stages {

        stage('Install Docker') {
      steps {
        sh 'chown jenkins: -R \$PWD/'
        sh 'apt-get update -y && apt-get install docker -y'
      }
    }
    
    stage('Checkout Source') {
      steps {
        git 'https://github.com/justmeandopensource/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
