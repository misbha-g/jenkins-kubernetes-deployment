pipeline {

  environment {
    dockerimagename = "devteam18/react-app"
    dockerImage = ""
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
          def kubeconfigCredential = credentials('kubernetes-configs')
          def kubeconfigFile = writeFile(file: 'kubeconfig', text: kubeconfigCredential)
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml", kubeconfigFile: kubeconfigFile)
        }
      }
    }

  }

}