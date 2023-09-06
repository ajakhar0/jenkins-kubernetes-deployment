pipeline {

  environment {
    dockerimagename = "amanjakharaptos/react-app"
    dockerImage = ""
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
                    // Make sure Minikube is running
                    sh 'minikube start'
                    
                    // Set the Minikube context
                    sh 'kubectl config use-context minikube'
                    
                    // Fetch your deployment file from the GitHub repository
                    sh "curl -o deployment.yaml https://github.com/ajakhar0/jenkins-kubernetes-deployment/raw/main/deployment.yaml"
                    
                    // Deploy your application to Minikube
                    sh 'kubectl apply -f deployment.yaml'
                }
      }   
    }

  }

}
