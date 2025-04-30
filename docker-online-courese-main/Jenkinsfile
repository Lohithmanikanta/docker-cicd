pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:$PATH" // Add Docker path to Jenkins environment
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Yaswanth-Marri/docker-online-courese.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t yaswanthmarri/online-course:latest .' // Use full path
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '/usr/local/bin/docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    }
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '/usr/local/bin/docker stop online-course-container || true'
                sh '/usr/local/bin/docker rm online-course-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh '/usr/local/bin/docker run -d -p 5001:80 --name online-course-container yaswanthmarri/online-course:latest'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
