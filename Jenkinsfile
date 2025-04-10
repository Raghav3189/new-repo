pipeline {
    agent any

    stages {
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG']) {
                    sh '''
                        export KUBECONFIG=${KUBECONFIG}
                        kubectl apply -f k8s/
                    '''
                }
            }
        }
    }
}
