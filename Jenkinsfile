pipeline {
    agent any
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/adikesavanaidug2404/valentinepro.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Valentine -Dsonar.projectName=Valentine"
                }
            }
        }
        stage('Build&Tag Docker Image') {
            steps {
                sh "docker build -t adikesavanaidug2404/valentine:v1 ."
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-token', toolName: 'docker') {
                        sh "docker push adikesavanaidug2404/valentine:v1"
                    }
                }
            }
        }
        stage('Deploy to Container') {
            steps {
                sh "docker run -d -p 8081:80 adikesavanaidug2404/valentine:v1"
            }
        }
    }
}
