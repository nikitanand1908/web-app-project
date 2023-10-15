pipeline {
    agent any
    
    tools {

        maven 'Maven'
    }

    environment {
        DOCKER_IMAGE = 'dockeradmin/tomcat:latest'
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
                    docker.build tomcat:latest
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('docker.io/library/tomcat:latest', '123545') {
                        docker.image(tomcat).push()
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
