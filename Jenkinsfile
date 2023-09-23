pipeline {

  environment {
    dockerimagename = "devteam18/react-app"
    dockerImage = ""
    KUBECONFIG = credentials('kube-config')
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

      stage('List the files') {
            steps {
                script {
                    sh 'ls'
                }
            }
        }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
            sh 'kubectl apply -f deployment.yaml'
        }
      }
    }

  }

}
