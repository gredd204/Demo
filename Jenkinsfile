pipeline {

    agent { node { label 'docker-maven-slave' }
          }
    
  
   environment {
    dockerimagename = "sandeepreddy1166/demorepo1"
    dockerImage = ""
  }

  stages {

       
    stage('Checkout Source') {
      steps {
        git 'https://github.com/gredd204/hcc-k8s-examples.git'
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
