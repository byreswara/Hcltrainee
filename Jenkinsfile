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
		tool name: 'maven', type: 'maven'
                sh 'mvn sonar:sonar -Dsonar.host.url=http://13.229.63.99:9000 -Dsonar.login=989544f9bd32add330bb1e8aae8e60cfef053c9c'
		}
	      }
          stage('deploy') {
            steps {
                sh 'cp /var/lib/jenkins/workspace/Hello-world/target/java-tomcat-maven-example.war /opt/tomcat-8.5/webapps/'
		}
	      }
            }
}
