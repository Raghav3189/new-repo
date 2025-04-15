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
                sh """
                    mvn clean install -DskipTests -Dcheckstyle.skip=true
                """
                echo 'Maven installed'
            }
        }

        stage('Code Test Analysis with SonarQube') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                script {
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey="spring-petclinic" \
                        -Dsonar.host.url=http://35.223.44.238:9000 \
                        -Dsonar.token=${env.SONAR_TOKEN}
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
