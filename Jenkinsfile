pipeline {
    agent any
	stages {
	  stage('build') {
            steps {
		tool name: 'maven', type: 'maven'
                sh 'mvn package'
		}
	      }
            }
}
