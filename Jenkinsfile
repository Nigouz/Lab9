pipeline {
	agent any
 stages {
 stage ('Checkout') {
 steps {
 git branch:'master', url: 'https://github.com/OWASP/Vulnerable-WebApplication.git'
 }
 
		stage('Build'){
			steps{
				echo "Current workspace is ${env.WORKSPACE}"
                sh 'docker-compose up --build -d' //Force rebuild images
				sh 'docker-compose ps'
			}
		}

		// //Common - ERROR: SonarQube server [ http://localhost:9000] can not be reached Change to your local computer ip address
		stage('Code Quality Check via SonarQube') {
			steps {
				script {
					def scannerHome = tool 'SonarQube';
					withSonarQubeEnv('SonarQube') {
						sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
					}
				}
			}
		}
	}
	post{
		always {
			echo 'Sucessfully generate the report ...'
			//SonarQube - Think report generation might be deprecated 
			recordIssues enabledForFailure: true, tool: sonarQube()
		}
	}
}
}
