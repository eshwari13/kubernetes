pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "eshwari13/g3-allergy-service:latest"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/eshwari13/kubernetes.git']]])     
            }
        }
      stage ('Build') {
          steps {
				 sh 'mvn clean install -DskipTests'             
            }
      }
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry 
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to dockerhub') {
     steps{  
         script {
                sh 'echo eshwari13 | docker login --eshwari13  --password-stdin '
                sh 'eshwari13/g3-allergy-service:latest'
         }
        }
      }

       stage('K8S Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'K8S', serverUrl: '']) {
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                }
            }
        }
       }
    }
}