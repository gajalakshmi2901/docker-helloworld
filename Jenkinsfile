pipeline {
    agent any
    parameters {
        string(name: "VM_USERNAME", defaultValue: "1CHAdministrator", description: "Enter the username for VM")
        string(name: "VM_PASSWORD", defaultValue: "Gajalakshmi@01", description: "Enter the password for Password")
        string(name: "VM_IP",  description: "Enter the remote server IP address")
        string(name: "ACR_SERVER", defaultValue: "projecte.azurecr.io", description: "Enter the server URL for ACR")
        string(name: "REGISTRY_NAME", defaultValue: "projecte", description: "Enter the registry username")
        string(name: "REGISTRY_PASSWORD", defaultValue: "o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS", description: "Enter the registry password")
        string(name: "REPO_CREDENTIALS", defaultValue: "Gajalakshmi-tf", description: "Enter the credential ID for Azure repo")
        string(name: "IMAGE_NAME", defaultValue: "image1", description: "Enter the image name")
        string(name: "TOKEN_ACR", defaultValue: "acr-secret6", description: "Enter the acr name")
        string(name: "TOKEN_PASSWORD", defaultValue:"GYvrR6y5g1cft8GD3BzKbyNLLsiUb+TFYsbr2hqL5v+ACRD/U3Nx", description: "Enter the token password")
        string(name: "SERVICE_NAME", defaultValue:"hello-world-svc", description: "Enter the service name")

    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: "${params.REPO_CREDENTIALS}", url: 'https://dev.azure.com/gajalakshmi0905/Terraform%20module/_git/gajalakshmi-tf', branch: 'develop'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh "docker build -t ${params.IMAGE_NAME} ."
                    sh "docker login ${params.ACR_SERVER} -u ${params.REGISTRY_NAME} -p ${params.REGISTRY_PASSWORD}"
                    sh "docker tag ${params.IMAGE_NAME} ${params.ACR_SERVER}/${params.IMAGE_NAME}"
                    sh "docker push ${params.ACR_SERVER}/${params.IMAGE_NAME}"
                }
            }
        }

        stage('Pulling Image from ACR and deploying ') {
            steps {
                script {
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'sudo docker login ${params.ACR_SERVER} -u ${params.REGISTRY_NAME} -p ${params.REGISTRY_PASSWORD}'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'sudo docker pull ${params.ACR_SERVER}/${params.IMAGE_NAME}:latest'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'git clone https://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@github.com/gajalakshmi2901/docker-helloworld.git'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl create secret docker-registry ${params.TOKEN_ACR} --docker-server=${params.ACR_SERVER} --docker-username=ACR-SECRETUSER --docker-password=${params.TOKEN_PASSWORD}'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl apply -f docker-helloworld/deployment.yaml'"
                }
            }
        }

        stage("Delay Stage") {
            steps {
                echo "Performing some tasks before exposing the container"
                sleep time: 60, unit: 'SECONDS'
                echo "Resuming pipeline"
            }
        }
        stage('Exposing Container') {
            steps {
                script {
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl expose deployment hello-world --name=${params.SERVICE_NAME} --type=NodePort --port=8080'"
                }
            }
        }

        stage('Port-Forward') {
            steps {
                script {
                
           sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl port-forward --address 0.0.0.0 service/${params.SERVICE_NAME} 8080:8080 > /dev/null 2>&1 &'"
                  
                }
            }
        }
    }
}
