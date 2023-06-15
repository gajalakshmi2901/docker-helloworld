pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Get the Azure DevOps credentials
                    withCredentials([usernamePassword(credentialsId: 'azure-repo-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "git config --global credential.helper '!f() { echo username=${USERNAME}; echo password=${PASSWORD}; }; f'"
                        sh "git clone --branch developer https://gajalakshmi0905@dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf"
                    }
                }
                
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Get the Azure Container Registry credentials
                    withCredentials([usernamePassword(credentialsId: 'acr-credentials', usernameVariable: 'ACR_USERNAME', passwordVariable: 'ACR_PASSWORD')]) {
                        sh 'docker build -t image1 .'
                        sh "docker login <your-acr-name>.azurecr.io -u ${ACR_USERNAME} -p ${ACR_PASSWORD}"
                        sh 'docker tag image1 <your-acr-name>.azurecr.io/image1:latest'
                        sh 'docker push <your-acr-name>.azurecr.io/image1:latest'
                    }
                }
            }
        }
    }
}
