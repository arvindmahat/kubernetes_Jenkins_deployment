pipeline {

    agent any
    
    environment {
        dockerimagename = "docker1196/sb2.0"
        registry = "docker1196/2.0"
        registryCredential = "Dockerhub_id"
        dockerImage = ''
    }
    
  
    stages {

    stage('Checkout Source') {
       steps {
         git branch: 'main', url: 'https://github.com/praveen1994dec/kubernetes_Jenkins_deployment.git'
       }
    }

           
    stage('Build Container') {
      steps {
        echo 'Building Container..'
                script {
                    def dockerHome = tool 'MyDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
      
    stage('Build B') {
             steps {
                 build job: "test", wait: true
                    }
    }    

    stage('Pushing Image') {
      
       steps{
         script {
           docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
           dockerImage.push("${env.BUILD_NUMBER}")            
           dockerImage.push("latest")        
           }
         }
       }
    }

    
  }

}
