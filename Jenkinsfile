pipeline {
    agent any

    tools {
        nodejs 'Node20'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/bayarmaa01/int_project.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                    bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t bayarmaa/todo-app:v1 ./todo-app'
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        bat 'docker push bayarmaa/todo-app:v1'
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                
                    bat 'docker-compose down'
                    bat 'docker-compose up --build -d'
                
            }
        }
    }

    post {
        success {
            echo ' Build and deployment successful!'
            emailext subject: " Jenkins Build Successful",
                     body: "Hello,\n\nThe Jenkins build and deployment for the ToDo App was successful.",
                     to: "b.bayarmaa0321@gmail.com"
        }

        failure {
            echo ' Build failed. Check logs for details.'
            emailext subject: " Jenkins Build Failed",
                     body: "Hi,\n\nThe Jenkins build failed. Please check the console logs for errors.",
                     to: "b.bayarmaa0321@gmail.com"
        }
    }
}
