pipeline {

  environment {
    dockerimagename = "murshidtp/flaskapp"
    dockerImage = ""
    KUBECONFIG = credentials('kuberneteskey')
    
  }

  agent any

  stages {

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhubkey'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying to Kubernetes') {
      steps {
        script {
          sh 'kubectl apply -f deployment.yml'
        }
      }
    }

  }

}
