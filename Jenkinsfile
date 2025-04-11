pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
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
                sh 'kubectl apply -f k8s/db.yml --validate=false'
                sh 'kubectl apply -f k8s/petclinic.yml --validate=false'
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

