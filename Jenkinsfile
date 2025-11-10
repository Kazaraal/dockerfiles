pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        IMAGE_NAME = 'your-dockerhub-username/your-image-name'
    }

    stages {
        stage('Checkout') {
            steps {
                git brach: 'master',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/Kazaraal/dockerfiles.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'echo "Running tests inside container"'
                        // Add your test commands here
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') 
                        {
                        dockerImage.push()
                        dockerImage.push('latest')
                        }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up Docker images'
            script {
                sh 'docker system prune -f'
            }
        }
    }
}