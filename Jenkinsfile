pipeline {
	agent any
	
	environment {
		MAJOR_VERSION = 1
	}
	
	stages {
		stage('Say Hello') {
			
			steps {
				sayHello 'Awesome Start!'
			}
		}
	  
		stage ('Git Information') {
			steps {
				echo "My Branch Name: ${env.BRANCH_NAME}"

				script {
					def myLib = new amitjain3112.git.gitStuff();
					
					echo "My Commit: ${myLib.gitCommit("${env.WORKSPACE}/.git")}"
			
				}
			}
		}
	}
}
