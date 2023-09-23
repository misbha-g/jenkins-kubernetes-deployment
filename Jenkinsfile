pipeline {

  environment {
    dockerimagename = "devteam18/react-app"
    dockerImage = ""
    KUBECONFIG = credentials('kubernetes')
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

      stage('Install kubectl') {
            steps {
                script {
                    sh 'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
                    sh 'chmod +x kubectl'
                    sh 'mv kubectl /usr/local/bin/'
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
