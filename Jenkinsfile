pipeline {

  environment {
    dockerimagename = "devteam18/react-app"
    dockerImage = ""
    KUBECONFIG = credentials('minikube-server-config')
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/misbha-g/jenkins-kubernetes-deployment.git'
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
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
            bat "kubectl apply -f deployment.yaml"
            bat "kubectl apply -f ingress.yaml"
        }
      }
    }

  }

}
