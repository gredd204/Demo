pipeline {

    agent { node { label 'docker-maven-slave' }
          }
    
  
   environment {
    dockerimagename = "sandeepreddy1166/demorepo1"
    dockerImage = ""
  }

  stages {

    stage('Git Clone') {
      steps {
        git credentialsId: 'Democred', url: 'https://github.com/gredd204/nodeapp_demo.git'
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
