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
        stage('deploy') {
            steps {
                sh 'cp /var/lib/jenkins/workspace/Hello-world/target/java-tomcat-maven-example.war /opt/tomcat-8.5/webapps/'
		}
	      }
           
            }
}
