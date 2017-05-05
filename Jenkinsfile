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
	  
		stage ('Branch-Build Information') {
			steps {
				echo "My Branch Name: ${env.BRANCH_NAME}"

				script {
					echo "My BUILD Number: ${env.BUILD_NUMBER}"
			
				}
			}
		}

		stage ('Promote Development branch to Master branch') {

			when {
				branch 'development'
			}
			steps {
				echo "Promote development code to Master branch"

				echo "Checking out Development branch"
				sh 'git checkout development'
				echo "Checking out Master branch"
				sh 'git pull origin'
				sh 'git checkout master'
				echo "Merging Development branch into Master branch"
				sh 'git merge --no-ff development'
				echo "Pushing to Origin Master"
				sh 'git push origin master'
				echo "Tagging the Release"
				sh "git tag pipeline-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
				sh "git push origin pipeline-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
			}
			post {
				success {
					emailext(
						subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Development Promoted to Master",
						body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Development Promoted to Master":</p>
						<p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
						to: "amit@localhost.com"
					)
				}
			}
		}
		}
		post {
		    	failure {
			      emailext(
			        subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
			        body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
			        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
			        to: "amit@localhost"
     				 )
    			}
		}
}
