pipeline {
    agent any
     tools {
       maven 'maven'
           }
	stages {
	  stage('build and sonar analysis') {
            steps {
		tool name: 'maven', type: 'maven'
                sh 'mvn clean package sonar:sonar'
		}
	      }
		
	      }
	  stage("Quality Gate status") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }	
          stage('deploy') {
            steps {
                sh 'cp /var/lib/jenkins/workspace/Hello-world/target/java-tomcat-maven-example.war /opt/tomcat-8.5/webapps/'
		}
	      }
            }
}
