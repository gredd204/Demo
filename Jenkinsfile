pipeline {  
  
  agent { node { label 'docker-maven-slave' }
            }
  
   environment {
    dockerimagename = "sandeepreddy1166/demorepo1"
    dockerImage = ""
  }

  stages {

       
    stage('Checkout Source') {
     // agent { node { label 'docker-maven-slave' }
      //      }
          
      steps {
         checkout scm

      }
    }
    

    stage('Build image') {
        
    // agent { node { label 'docker-maven-slave' }
    //        }
                  
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockercred'
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
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "K8scred")
        }
      }
    }
  }
}
