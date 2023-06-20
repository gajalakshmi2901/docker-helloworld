pipeline {
    agent any
    parameters {
        string(name: "VM_USERNAME", defaultValue: "1CHAdministrator", description: "Enter the username for VM")
        string(name: "VM_PASSWORD", defaultValue: "Gajalakshmi@01", description: "Enter the password for Password")
        string(name: "VM_IP", defaultValue: "20.96.41.90", description: "Enter the remote server IP address")
        string(name: "DOCKER_SERVER", defaultValue: "projecte.azurecr.io", description: "Enter the server URL for ACR")
        string(name: "REGISTRY_NAME", defaultValue: "projecte", description: "Enter the registry username")
        string(name: "REGISTRY_PASSWORD", defaultValue: "o0cledTfrzC8ChAaJCGF5l0fsvmRWQCGQ4Yrhve97G+ACRCLXnSS", description: "Enter the registry password")
        string(name: "REPO_CREDENTIALS", defaultValue: "Gajalakshmi-tf", description: "Enter the credential ID for Azure repo")
        string(name: "IMAGE_NAME", defaultValue: "image1", description: "Enter the image name")
        string(name: "DEPLOYMENT_FILE", defaultValue: "deployment", description: "Deployment file name")
        string(name: "SERVICE_NAME", defaultValue: "hello-world-svc", description: "Enter the service name")
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
                    sh "docker login ${params.DOCKER_SERVER} -u ${params.REGISTRY_NAME} -p ${params.REGISTRY_PASSWORD}"
                    sh "docker tag ${params.IMAGE_NAME} ${params.DOCKER_SERVER}/${params.IMAGE_NAME}"
                    sh "docker push ${params.DOCKER_SERVER}/${params.IMAGE_NAME}"
                }
            }
        }

        stage('Pulling Image from ACR and deploying pod') {
            steps {
                script {
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'sudo docker login ${params.DOCKER_SERVER} -u ${params.REGISTRY_NAME} -p ${params.REGISTRY_PASSWORD}'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'sudo docker pull ${params.DOCKER_SERVER}/${params.IMAGE_NAME}:latest'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'git clone https://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@github.com/gajalakshmi2901/docker-helloworld.git'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl delete service ${params.SERVICE_NAME} --ignore-not-found=true'"
                    sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'kubectl apply -f docker-helloworld/${params.DEPLOYMENT_FILE}.yaml'"
                }
            }
        }

        stage("Delay Stage") {
            steps {
                echo "Performing some tasks before exposing the container"
                sleep time: 200, unit: 'SECONDS'
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
                    try {
                        sh "sshpass -p '${params.VM_PASSWORD}' ssh -o 'StrictHostKeyChecking=no' -p 50022 ${params.VM_USERNAME}@${params.VM_IP} 'sudo kubectl port-forward --address 0.0.0.0 service/${params.SERVICE_NAME} 8080:8080 > /dev/null 2>&1 &'"
                    } catch(Exception e) {
                        echo 'Success'
                    }
                }
            }
        }
    }
}
