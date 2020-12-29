pipeline {
    agent any
    tools {
      maven 'Maven'
    }
    stages {
      stage ('Initialize') {
        steps {
          sh '''
                  echo "PATH = ${PATH}"
                  echo "M2_HOME = ${M2_HOME}"
             '''
        }
      }
      stage ('Build') {
        steps {
          sh 'mvn clean package'
        }  
      }
      stage ('Deploy to Tomcat') {
        steps {
          sshagent(['tomcat']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.204.70.210:/prod/apache-tomcat-8.5.61/webapps/webapp.war'
          }
        }
      }
    }
}
