pipeline {
    agent any

    environment {
        REGISTRY = 'ghcr.io/raghav3189/new-repo'  // Your Docker/OCI registry path
        KUBECONFIG_CREDENTIALS_ID = 'kubeconfig'  // Your existing Jenkins credential ID
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Raghav3189/new-repo.git'  // Your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.REGISTRY}:latest")
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: "${env.KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                    '''
                }
            }
        }
    }
}
