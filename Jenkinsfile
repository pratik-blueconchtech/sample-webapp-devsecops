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
      stage ('Check-Git-secrets') {
        steps {
          sh 'rm trufflehog || true'
          sh 'docker pull gesellix/trufflehog'
          sh 'docker run gesellix/trufflehog --json https://github.com/harsha698/sample-webapp-devsecops.git > trufflehog'
          sh 'cat trufflehog'
        }
      }
      stage ('Source-Composition-Check') {
        steps {
          sh 'rm owasp* || true'
          sh 'wget "https://github.com/harsha698/sample-webapp-devsecops/blob/master/owasp-dependency-check.sh"'
          sh 'chmod +x owasp-dependency-check.sh'
          sh 'bash owasp-dependency-check.sh'
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
