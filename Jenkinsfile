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
            }
}
