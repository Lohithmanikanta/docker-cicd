pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = "lohith066/online-course"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Lohithmanikanta/docker-cicd.git'
            }
        }

        stage('Build') {
            steps {
                dir('docker-online-courese-main') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
                bat "docker tag %IMAGE_NAME%:%IMAGE_TAG% %IMAGE_NAME%:latest"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                bat '''
                echo|set /p=%DOCKERHUB_CREDENTIALS_PSW%|docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
                bat "docker push %IMAGE_NAME%:latest"
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop online-course-container || exit 0'
                bat 'docker rm online-course-container || exit 0'
            }
        }

        stage('Run New Container') {
            steps {
                bat "docker run -d -p 5001:80 --name online-course-container %IMAGE_NAME%:latest"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment step placeholder - customize based on your environment.'
            }
        }
    }

    post {
        always {
            bat 'docker logout'
            bat "docker rmi %IMAGE_NAME%:%IMAGE_TAG% || exit 0"
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
