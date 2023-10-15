pipeline {
    agent any
    
    tools {

        maven 'Maven'
    }

    environment {
        DOCKER_IMAGE = 'dockeradmin/tomcat:8-jre8'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Package with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build and Publish Docker Image') {
            steps {
                script {
                    docker.build DOCKER_IMAGE
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', '123545') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy to Docker') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).withRun('-p 8080:8080 -d')
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded. Application deployed to Docker container.'
        }
        failure {
            echo 'Pipeline failed. Deployment to Docker container was not successful.'
        }
    }
}
