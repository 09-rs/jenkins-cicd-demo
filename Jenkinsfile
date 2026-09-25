pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t jenkins-cicd-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f jenkins-cicd-demo || true
                    docker run -d \
                      --name jenkins-cicd-demo \
                      jenkins-cicd-demo:${BUILD_NUMBER}
                '''
            }
        }
    }
}
