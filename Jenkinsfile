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
               registryCredential = 'DockerCredpri'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

      stage('Deploy to Kubernetes nonprod'){

   	steps {
   		script {

   				glKubernetesApply credentials: "K8scred",
   				cluster: "ctcnonprdusr001",
   				namespace: "iva-dev01",
   				yamls: ["Deployment_Nonprod.yml"],
   				isProduction: false,
   				env: "nonprod",
   				deleteIfExists: true, wait: false

   			}
   		}
   	}
}
  }
}
