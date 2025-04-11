pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig') // Replace with your Jenkins secret ID for kubeconfig
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '📦 Deploying to Kubernetes...'
                sh 'kubectl apply -f k8s/db.yml'
                sh 'kubectl apply -f k8s/petclinic.yml'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}

