pipeline {

  environment {
    dockerimagename = "amanjakharaptos/react-app"
    dockerImage = ""
    KUBECONFIG = credentials('test-minikube')
  }

  agent any

  stages {
    
    
    stage('Build image') {
      steps{
        script {
          env.dockerImage = docker.build env.dockerimagename
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
                    sh 'kubectl --kubeconfig=$KUBECONFIG'
                    
                    // Fetch your deployment file from the GitHub repository
                    sh "curl -o deployment.yaml https://github.com/ajakhar0/jenkins-kubernetes-deployment/raw/main/deployment.yaml"
                    
                    // Deploy your application to Minikube
                    sh 'kubectl apply -f deployment.yaml'
                }
      }   
    }

  }

}
