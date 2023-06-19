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

            stage('Pulling Image from ACR') {
                steps{
                    script{
                        sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "sudo docker login projecte.azurecr.io -u projecte -p o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS"'
                        sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "sudo docker pull projecte.azurecr.io/image1:latest"'

                }
        }
             stage('Deploying pod and exposing on container') {
             steps {
                  script {
                    sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90  "sudo yum -y install git"'
                    sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90  "git clone https://github.com/gajalakshmi2901/docker-helloworld.git"'
                     sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90  "kubectl apply -f docker-helloworld/deployment.yaml"'
                     sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "kubectl expose deployment hello-world --name=hello-world-svc --type=NodePort --port=8080"'
                     sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "sudo kubectl create secret docker-registry acr-secret --docker-server=projecte.azurecr.io --docker-username=projecte --docker-password=o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS'

                    
                }
            }
        }
        stage('Port-Forward') {
            steps {
                script {
                    try{

                    sh 'sshpass -p "Gajalakshmi@01" ssh -o "StrictHostKeyChecking=no" -p 50022 1CHAdministrator@20.96.41.90 "sudo kubectl port-forward --address 0.0.0.0 service/hello-world-svc 8080:8080 > /dev/null 2>&1 &"'
                    }
                    catch(Exception e){
                        echo 'Success'
                    }
                }
            }
        }
    }
}