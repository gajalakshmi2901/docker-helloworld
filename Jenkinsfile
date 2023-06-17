pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps{
                git credentialsId: 'Gajalakshmi-tf', url: 'https://dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf', branch: 'develop'
                
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
            stage('Pulling and Running Image') {
                steps{
                    script{
                        sh 'sudo usermod -aG docker $USER'
                        sh 'sudo chmod 666 /var/run/docker.sock'
                        sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "docker login projecte.azurecr.io -u projecte -p o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS"'
                        sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "docker pull projecte.azurecr.io/image1"'
                        sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "docker run -d -p 8080:80 image1:latest"'
                    }
                }
        }
    }
}