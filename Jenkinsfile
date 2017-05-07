pipeline {
	agent any
	
	environment {
		MAJOR_VERSION = 1
		OUTPUT_DIR = '/mnt1/jenkins-output'
		VENV_DIR = "venv${env.BUILD_NUMBER}"
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
					sh "cd ${env.WORKSPACE}"
					sh 'git log --oneline'
				}
			}
		}

		stage ('Initialize PyBuilder for Building Python Source code') {
			steps {
				echo "Building Python Project using PyBuilder"

				echo "Create virtualenv to install Build env separate from default Python" 

				sh 'mkdir ${OUTPUT_DIR}/${VENV_DIR}"
				sh "virtualenv ${OUTPUT_DIR}/${VENV_DIR}/venv" 
				sh "source ${OUTPUT_DIR}/${VENV_DIR}/venv/bin/activate"

				echo "Now install pybuilder"

				sh 'pip install pybuilder'
			}

			steps {
				echo "Add source files into src/main/scripts folder"

				sh "cp ${env.$WORKSPACE}/csv_split.py ${OUTPUT_DIR}/${VENV_DIR}/src/main/scripts"
				sh "cp ${env.$WORKSPACE}/execution_time.py ${OUTPUT_DIR}/${VENV_DIR}/src/main/scripts"

				echo "Add unittest files into src/main/python"

				sh "cp ${env.$WORKSPACE/execution_time_tests.py ${OUTPUT_DIR}/${VENV_DIR}/src/unittest/python/"
				sh "cp ${env.$WORKSPACE/csv_split_tests.py ${OUTPUT_DIR}/${VENV_DIR}/src/unittest/python/"

				echo "Finally add build.py into virtual env to build"

				sh "cp ${env.$WORKSPACE}/build.py ${OUTPUT_DIR}/${VENV_DIR}/" 
			}

			steps{
				echo "From Pybuilder install dependencies"
				sh 'pyb install_dependencies'
			}

		}

		stage ('Run Unit Test'){
		
			echo "Code for running UnitTests on Source Code"
			echo "If UnitTests are successful,build the source code"

		}

		stage ('BUILD Python Source code'{
			
			echo "Now Build the source code if UnitTests are successful"
			echo "After successful build packaged build will be moved to Green folder"

		}

		stage ('Move stable build to Green'){
		
			echo "Code for Copying stable Package to Green folder"
			echo "Packaged code will be deployed to Production servers from Green folder only"

		}

		stage ('Deploy Stable Build'){

			echo "Code for deploying stable build to production hosts using Ansible"
			echo "Successful package from Green folder only will be deployed to Production server"
	
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
