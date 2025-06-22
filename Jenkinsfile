pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'DockerHub'
        IMAGE_NAME = 'devopsabhishekh/frontend-angular-19'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/ItSpecialistDevOps/Jenkinsfile.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('frontend-angular-19') {
                    script {
                        docker.build("${IMAGE_NAME}", '.')
                    }
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                      echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}").push('latest')
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
