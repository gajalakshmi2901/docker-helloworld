pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Get the Azure DevOps credentials
                    withCredentials([usernamePassword(credentialsId: 'gajarepo')]) {
                        sh "git config --global credential.helper '!f() { echo username=${USERNAME}; echo password=${PASSWORD}; }; f'"
                        sh "git clone --branch develop https://gajalakshmi0905@dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf"
                    }
                }
                
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                        sh 'docker build -t image1 .'
                    }
                }
            }
        }
    }
