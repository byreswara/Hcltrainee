pipeline {
    agent any
// TOOLS DECLARATION //
     tools {
       maven 'maven'
           }
// BUILD STAGE START //
	stages {
	  stage('compile') {
            steps {
		tool name: 'maven', type: 'maven'
                sh 'mvn clean compile'
		}
	      }
// SONAR ANALYSIS STAGE START //
	/*stage('sonar') {
            steps {
             tool name: 'maven', type: 'maven'
             sh 'mvn sonar:sonar  -Dsonar.host.url=http://13.229.63.99:9000 -Dsonar.login=989544f9bd32add330bb1e8aae8e60cfef053c9c'
            }
	      }
	 */
	  stage("SonarQube analysis") {
               steps {
              withSonarQubeEnv('sonar') {
                 sh 'mvn sonar:sonar'
		  // sh 'sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=src'
              }    
          }
      }

//SOANR QUALITY GATES STAGE START //
		
	/*stage("Quality Gate status") {
	  steps {
	    script {
                def qualitygate = waitForQualityGate()
                if (qualitygate.status != "OK") {
                  error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"	
		}
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
	  
	  */
//PACKAGING STAGE //
		
	stage('package') {
            steps {
		tool name: 'maven', type: 'maven'
                sh 'mvn package'
		}
	      }	
//END PACKAGING STAGE //
		
// APPROVAL STAGE USING INPUT //

	/*  stage ('Deploy To Prod'){
                //input{
                 //  message "Do you want to proceed for production deployment?"
                //  }
		input message: 'Deploy to Production', ok: 'Continue',
                                parameters: [choice(name: 'APPROVE_PROD', choices: 'YES\nNO', description: 'Deploy from STAGING?')]
              steps { sh 'echo "Deploy into Prod"'}
                
              }
	*/

// APPROVAL STAGE USING SLACK NOTIFICATION //
   //1st  try	//
	 stage('Slack notification'){
            steps {
              /*slackSend channel: '#devops', 
		      color: 'good', 
		      message: 'slackSend "started ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"', 
		      teamDomain: 'hcl-emb5598', tokenCredentialId: 'slack'
		 */
    // 2nd try  //
	     slackSend (baseUrl: "https://hcl-emb5598.slack.com/",
		        channel: "#devops", 
			color: "#4286f4", 
			message: "Deploy Approval: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}/input)")
                script {
                    try {
                        timeout(time:30, unit:'MINUTES') {
                            env.APPROVE_PROD = input message: 'Deploy to Production', ok: 'Continue',
                                parameters: [choice(name: 'APPROVE_PROD', choices: 'YES\nNO', description: 'Deploy from STAGING?')]
                            if (env.APPROVE_PROD == 'YES')
				{
                                env.DPROD = true
                                } 
			    else
			       {
                                env.DPROD = false
                                }
                        }
                    } catch (error) {
                        env.DPROD = true
                        echo 'Timeout has been reached! Deploy to STAGING automatically activated'
                    }
                }
       // 2nd try end  //
            }
        }
// END APPROVAL STAGE USING SLACK NOTIFICATION //

//DEPLOYMENT SRAGE//
    /* stage('deploy') {
            steps {
                sh 'cp /var/lib/jenkins/workspace/Hello-world/target/java-tomcat-maven-example.war /opt/tomcat-8.5/webapps/'
		}
	      }
	  */
//END DEPLOYMENT SRAGE//
		
//START REMOTE DEPOYMENT STAGE //

	   stage("deploy to remote server") {
            steps {
		sshPublisher(publishers: [sshPublisherDesc(configName: 'Tomcat', 
							   transfers: [sshTransfer(cleanRemote: false, 
							   excludes: '', execCommand: '', 
							execTimeout: 120000, 
							flatten: false, 
							makeEmptyDirs: false, 
							noDefaultExcludes: false, 
					patternSeparator: '[, ]+', 
					remoteDirectory: '', 
				remoteDirectorySDF: false, removePrefix: 'target', 
			sourceFiles: 'target/*.war')], 
                            usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	    }

		   
   // START POST BUILD EMAIL NOTIFICATION //
	post {
         always {
		 mail bcc: '', body: 'Build # $env.BUILD_NUMBER', cc: '', from: '', replyTo: '', subject: 'jenkins job', to: 'byreswar@gmail.com'
	//mail bcc: '', body: 'this is jenkins job info' '$env.PROJECT_NAME - Build # $env.BUILD_NUMBER - $env.BUILD_STATUS:' , cc: '', from: '', replyTo: '', subject: 'jenkins job' '$env.PROJECT_NAME - Build # $env.BUILD_NUMBER - $env.BUILD_STATUS!', to: 'byreswar@gmail.com' 
	//emailext attachLog: true, body: '$env.PROJECT_NAME - Build # $env.BUILD_NUMBER - $env.BUILD_STATUS:', subject: '$env.PROJECT_NAME - Build # $env.BUILD_NUMBER - $env.BUILD_STATUS!', to: 'byreswar@gmail.com'
             }
           }
   
        }
 //END START REMOTE DEPOYMENT STAGE //	
     }
}
