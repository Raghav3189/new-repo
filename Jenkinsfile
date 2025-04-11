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
                    kubectl apply -f k8s/db.yml --validate=false
                    kubectl apply -f k8s/petclinic.yml --validate=false
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

