pipeline {

    agent any
    
    environment {
        dockerimagename = "docker1196/sb2.0"
        registry = "docker1196/myspc"
        registryCredential = "Dockerhub_id"
        dockerImage = ''
    }
  
  
    stages {

    stage('Checkout Source') {
       steps {
         git 'https://github.com/praveen1994dec/kubernetes_Jenkins_deployment.git'
       }
    }

    stage('Build package') {
       steps {
         sh 'mvn clean package '
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
                 build job: "Sonar_Project", wait: true
                    }
    }    

    stage('Pushing Image') {
       environment {
                registryCredential = 'dockerhubcred'
            }
       steps{
         script {
           docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
           dockerImage.push("${env.BUILD_NUMBER}")            
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
