pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS'
    }

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'cd backend && npm install'
                bat 'cd frontend && npm install'
            }
        }

        stage('Build Frontend') {
            steps {
                bat 'cd frontend && npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t prakritiagrawal/pms-backend:latest ./backend'
                bat 'docker build -t prakritiagrawal/pms-frontend:latest ./frontend'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'prakritiagrawal', passwordVariable: 'Rit3895h@')]) {
                    bat 'docker login -u %prakritiagrawal% -p %Rit3895h@%'
                    bat 'docker push prakritiagrawal/pms-backend:latest'
                    bat 'docker push prakritiagrawal/pms-frontend:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker-compose up -d'
            }
        }

        stage('Health Check') {
            steps {
                bat 'timeout /t 30'
                bat 'docker ps'
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}