pipeline {
  environment {
    dockerimagename = "yasirdocker/web-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/git-yasir/jenkins.git' 
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying WEB-APP container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml")
        }
      }
    }
  }
}
