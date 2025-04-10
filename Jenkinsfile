pipeline {
    agent any
    
    stages {
        stage('Test Trigger') {
            steps {
                echo "GitHub webhook triggered this build successfully!"
                sh 'echo "Build Number: ${BUILD_NUMBER}"'
                sh 'echo "Git Commit: ${GIT_COMMIT}"'
            }
        }
    }
}
