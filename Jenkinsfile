pipeline {
    agent any
    tools { 
        maven 'Maven' 
   }
environment {
        SSH_CREDENTIALS = credentials('a9ba75cbd9f843548eb9cefdbd119253')
    }
    stages {
          stage ('compile') {
            steps {
                sh "mvn compile" 
            }
         }
          stage ('test') {
            steps {
                sh "mvn test" 
            }
         }
          stage ('package') {
            steps {
                sh "mvn package" 
            }
         }
          stage ('Build') {
            steps {
                sh "mvn install" 
            }
         }
        stage('Deploy to Tomcat') {
            steps {
                script {
                    sshagent(['$SSH_CREDENTIALS']) {
                        sh 'ssh ec2-user@18.183.176.95 "/home/ec2-user/tomcat/bin/shutdown.sh"'
                        sh 'scp target/web-app-project.war ec2-user@18.183.176.95:/home/ec2-user/tomcat/webapps'
                        sh 'ssh ec2-user@18.183.176.95 "/home/ec2-user/tomcat/bin/startup.sh"'
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }

    }

