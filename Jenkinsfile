pipeline {
  agent any
  
  environment {
    dockerImage = ""
    registryCredential = 'dockerhub-credentials'
  }
  
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/astifoman/jenkins-kubernetes-deployment.git'
      }
    }
    
    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build("bravinwasike/react-app")
        }
      }
    }
    
    stage('Pushing Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }
    
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }
  }
}
