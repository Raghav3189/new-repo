pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build, Install and Deploy with Maven') {
            steps {
                sh """
                    mvn clean install
                """
                echo 'Maven build and install completed'

                sh """
                    mvn clean deploy 
                """
                echo 'Maven deploy completed'
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
                        -Dsonar.host.url=http://34.55.173.9:9000 \
                        -Dsonar.token=${env.SONAR_TOKEN} \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
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
