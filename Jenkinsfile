

pipeline {
	/* A declarative Pipeline */
	agent any 		
	
	stages {
		stage('Build'){
			steps {
				sh '/Users/acherkas/Mypart/Jenkins_learn/apache-maven-3.5.2/bin/mvn clean package'
			}

			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage('Deploy to stage'){
			steps{
				build job: 'deploy-to-staging'
			}
		}
		stage('Deploy to Prod'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message:'Approve PROD deployment?'
				}
			build job: 'deploy-to-prod'
			}
		post{
			success {
				echo 'Succesfully deployed to PROD'
			}
			failure {
				echo ' Failed ('
			}
		}
		}
	}
}
