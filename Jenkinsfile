pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                // sh 'mvn clean install -DskipTests'
                echo 'Maven installed'
            }
        }

        stage('Code Test Analysis with SonarQube') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token') // Assuming you've stored the token in Jenkins
            }
            steps {
                script {
                    // Run SonarQube analysis
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=Spring PetClinic \
                        -Dsonar.host.url=http://35.223.44.238:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
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

