pipeline {
    agent any

    stages {
        stage('Initialize') {
            steps {
                echo '✅ Starting the pipeline...'
            }
        }

        stage('Build') {
            steps {
                echo '🔧 Building the project (mock)...'
                sh 'sleep 2'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests (mock)...'
                sh 'sleep 1'
            }
        }

        stage('Done') {
            steps {
                echo '🎉 Pipeline finished successfully!'
            }
        }
    }
}

