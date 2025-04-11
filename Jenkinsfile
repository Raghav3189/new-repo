pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '📦 Deploying to Kubernetes...'
                sh '''
                    export KUBECONFIG=$HOME/.kube/config
                    kubectl apply -f k8s/db.yml
                    kubectl apply -f k8s/petclinic.yml
                '''
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

