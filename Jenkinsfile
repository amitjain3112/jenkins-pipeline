pipeline {
	agent any
	
	environment {
		MAJOR_VERSION = 1
	}
	
	stages {
		stage('Say Hello') {
			
			steps {
				echo 'Awesome Start!'
			}
		}
	  
		stage ('Git Information') {
			steps {
				echo "My Branch Name: ${env.BRANCH_NAME}"

				script {
					
					echo "My BUILD Number: ${env.BUILD_NUMBER}"
			
				}
			}
		}
	}
}
