pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "raghav3168/new-repo:build-${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build, Install and Deploy with Maven') {
            steps {
                sh 'mvn clean install'
                echo 'Maven build and install completed'

                sh 'mvn deploy'
                echo 'Maven deploy completed'
            }
        }

        stage('Code Test Analysis with SonarQube') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey="spring-petclinic" \
                        -Dsonar.host.url=http://34.55.173.9:9000 \
                        -Dsonar.token=${SONAR_TOKEN} \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}
                    """
                }
            }
        }

        stage('Deploy Kubernetes Manifests') {
            steps {
                sh '''
                    gcloud auth activate-service-account --key-file=/var/lib/jenkins/gcp-creds/gcp-key.json
                    gcloud config set project trusty-wares-454707-d7
                    gcloud container clusters get-credentials my-cluster-1 --zone us-east1-b --project trusty-wares-454707-d7
                    kubectl apply -f k8s/
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get all'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
