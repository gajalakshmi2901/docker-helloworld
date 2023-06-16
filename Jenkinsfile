pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps{
                git credentialsId: 'gajarepo', url: 'https://dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf'

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
