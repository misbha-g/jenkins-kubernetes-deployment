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
          def kubeconfigCredential = credentials('kubernetes-configs') // Replace with the actual credential ID
          def kubeconfigFile = writeFile(file: 'kubeconfig', text: kubeconfigCredential)

          def kubernetesDeployOptions = [
            kubeconfigFile: kubeconfigFile,
            configs: 'deployment.yaml,service.yaml',
            enableConfigSubstitution: true,
            namespace: 'default',
          ]

          step([$class: 'KubernetesDeploy', configType: 'FILES', configs: 'kubeconfig', **kubernetesDeployOptions])
        }
      }
    }

  }

}