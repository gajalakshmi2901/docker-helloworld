pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps{
                git credentialsId: 'gajalakshmi', url: 'https://dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf', branch: 'develop'
                
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    
                       sh 'docker build -t image1 .'
                       sh 'docker login projecte.azurecr.io -u projecte -p o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS'
                       sh 'docker tag image1 projecte.azurecr.io/image1'
                       sh 'docker push projecte.azurecr.io/image1'
                    }
                }
            }
        }
    }
