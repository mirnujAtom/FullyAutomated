pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: 'ec2-52-25-197-82.us-west-2.compute.amazonaws.com', description: 'dev instance')
		string(name: 'tomcat_prod', defaultValue: 'ec2-52-33-21-152.us-west-2.compute.amazonaws.com', description: 'prod instance')
	}

	triggers {
		pollSCM('* * * * *')
	}

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

		stage ('Deplouyments'){
			parallel{
				stage ('Deploy to Staging'){
					steps {
						sh "scp -i /Users/Shared/Jenkins/.ssh/oregon_my.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
					}
				}
				stage ("Deploy to Production"){
	                steps {
	                    sh "scp -i /Users/Shared/Jenkins/.ssh/oregon_my.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
	                }
	            }
			}
		}
	}
}