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
				   deploy adapters: [tomcat8(credentialsId: 'd238d15f-fea2-4eb4-b038-22ec521f995f', path: '', url: 'http://ec2-18-220-141-64.us-east-2.compute.amazonaws.com:8888/')], contextPath: null, onFailure: false, war: ''
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




















