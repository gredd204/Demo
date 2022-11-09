#!/usr/bin/env groovy


@Library("com.optum.jenkins.pipeline.library@master") _

pipeline {  
  
  agent { node { label 'docker-maven-slave' }
            }
  
   environment {
    dockerimagename = "sandeepreddy1166/demorepo1"
    dockerImage = ""
    KUBECTL_VERSION = '1.14.2'
    K8S_CREDENTIALS_ID = 'K8scred'
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
   		    // git branch: 'master', url: 'https://github.optum.com/brieger/pipeline_test.git'
   				glKubernetesApply credentials: "$env.K8s_CREDENTIALS_ID",
          clusterPort: 443,
   				cluster: "hcc-ctc-prd-usr001-0.optum.com",
   				namespace: "iva-dev01",
   				yamls: ["Deployment.yml"],
   				isProduction: false,
   				env: "nonprod",
   				deleteIfExists: true, wait: false

   			
   		}
   	}

  }
}
