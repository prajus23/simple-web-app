pipeline {
   
    agent any 

    tools {
         maven  "Maven"
    }

    stages {

          stage("Preparation") {
                 steps {             	
					  echo "Preparing an applicaction using maven"
                      sh 'mvn clean package'	
                 }
                 post {
	                     success {
                              echo "Artifact is generated successfully."
							  echo "Archiving an artifact"
                              archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                         }
                         failure {
							echo "Failed to generate an artifact"
                         }
                 }
          } 

          
			stage("Build to staging") {
				steps {
				   deploy adapters: [tomcat8(credentialsId: 'e849c2bd-6021-4a30-a40c-1571e4121219', path: '', url: 'http://ec2-18-220-141-64.us-east-2.compute.amazonaws.com:8888/')], contextPath: null, onFailure: false, war: '**/*.war'
				}																								
				post {
				    success {
						echo "Application is deployed successfully on Tomcat in stanging environment"
					}
					failure {
						echo "Failed to deploy an application on Tomcat in staging environment"
					}
				}
			}
			
			stage("Perform code analysis") {
				steps {
				   sh 'mvn checkstyle:checkstyle'			   
				}
				post {
					success {
						checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
					}
				}
			}
		
	
	
	  
 }
	
	
}




















