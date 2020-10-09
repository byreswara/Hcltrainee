pipeline {
    agent any
     tools {
       maven 'maven'
           }
	stages {
	  stage('build') {
            steps {
		tool name: 'maven', type: 'maven'
                sh 'mvn package'
		}
	      }
          stage('sonar') {
            steps {
		withSonarQubeEnv('My SonarQube Server') {
		tool name: 'maven', type: 'maven'
                sh 'mvn sonar:sonar'
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
