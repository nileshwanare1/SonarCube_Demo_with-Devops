pipeline {

    agent any

    environment {
        IMAGE_NAME = "static-web"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nileshwanare1/SonarCube_Demo_with-Devops.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {

                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('SonarQube') {

                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=StaticWeb \
                        -Dsonar.sources=. \
                        -Dsonar.sourceEncoding=UTF-8
                        """

                    }
                }
            }			
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker stop static-container || true
                docker rm static-container || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker run -d \
                --name static-container \
                -p 80:80 \
                static-web
                '''
            }
        }
    }
}