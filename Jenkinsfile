pipeline {
    agent any
    tools { 
        maven 'Maven' 
   }
environment {
        SSH_CREDENTIALS = credentials('be8cb450-d739-4d6b-96b3-84c0d4672e91')
    }
    stages {
          stage ('compile') {
            steps {
                bat "mvn compile" 
            }
         }
          stage ('test') {
            steps {
                bat "mvn test" 
            }
         }
          stage ('package') {
            steps {
                bat "mvn package" 
            }
         }
          stage ('Build') {
            steps {
                bat "mvn install" 
            }
         }
stages {
        stage('Deploy to Tomcat') {
            steps {
                script {
                    sshagent(['$SSH_CREDENTIALS']) {
                        sh 'ssh ec2-user@52.192.134.122 "/home/ec2-user/tomcat/bin/shutdown.sh"'
                        sh 'scp target/web-app-project.war ec2-user@52.192.134.122:/home/ec2-user/tomcat/webapps'
                        sh 'ssh ec2-user@52.192.134.122 "/home/ec2-user/tomcat/bin/startup.sh"'
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
}
