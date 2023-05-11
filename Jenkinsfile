pipeline {  

  environment {
    dockerimagename = "kirangothe/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {    
    
//   stage('Install Docker') {
//       steps {
//         sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
//         sh 'sh get-docker.sh'
//         sh 'sudo usermod -aG docker $USER'
//       }
//     }

    stage('Checkout Source') {
      steps {
       git branch: 'main', url: 'https://github.com/kirangothe/Jenkins-kubernetes.git'
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
