pipeline {

	//agent { docker { image 'maven:3.6.3'} }
	agent any

	environment {
		dockerHome = tool 'dockerDefault'
		MSTEAMS_HOOK = "https://outlook.office.com/webhook/28226550-c439-44f5-89ea-6c842feec9ac@573b4a72-decb-41d8-b48b-5a25b51b31b4/JenkinsCI/70759a0f5304402cb36ff57c27ebf3de/ad108b75-24d3-456b-9b11-5e59132e6768"
		PROJECT_ROOT = "."
		PACKAGE_JSON_PATH = "${PROJECT_ROOT}/package.json"
		VERSION = "0.0.1"
	}

	stages {

		stage('Check Environment') {
			steps {
				echo "Build "
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
				echo "VERSION - $VERSION"
				echo "BRANCH_NAME - $env.BRANCH_NAME"

				script {
					def packageJSON = readJSON file: PACKAGE_JSON_PATH
					echo "version before: $VERSION"
					VERSION = packageJSON['version']
					echo "version after: $VERSION"
				}
				
			}
			
		}

		stage('deploy1') {
			steps {
				echo "deploy1 versionPack: $VERSION"
				sh 'echo version var ${VERSION}'

				sh "echo version local ${VERSION}"
			}
		}

		
		stage('Compile') {
			steps {
				echo "compile true"
			}
		}

		/*
		stage('Tests') {
            parallel {

				stage('Unit Test') {
					steps {
						sh "mvn clean test"
						*/
						//junit "**/target/surefire-reports/TEST-*.xml"
						/*
					}
				}

				stage('Integration Test') {
					steps {
						//sh "mvn failsafe:integration-test failsafe:verify"
						sh "exit 0"
						print "Teste de integração concluído"
					}
				}
        	}
		}

		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
				echo "Packaged"
			}
		}

		stage('Build Docker Image ') {
			steps {
				script {
					dockerImage = docker.build("matmedeiros/currency-exchange-devops:${VERSION}")
				}
			}
		}

		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry('','dockerHub') {
						dockerImage.push();
					}
				}
			}
		}

		*/



		
		

	}

	post {
		always {
			echo 'Finished'
			//deleteDir() /* clean up our workspace */
		}
		success {
			echo 'Build Successful :)'

			office365ConnectorSend (
				status: "${currentBuild.currentResult}",
				webhookUrl: "${MSTEAMS_HOOK}",
				color: '00ff00',
				message: "Build Successful: ${JOB_NAME} - ${BUILD_DISPLAY_NAME} " 
						   + "<br>Branch - ${env.BRANCH_NAME} " 
						   + "<br>Last Commiter: ${gitInfo('name')} - ${gitInfo('email')}" 
						   + "<br>Last Commit Message: ${gitInfo('message')}"
						   + "<br>Pipeline duration: ${currentBuild.durationString}"
			)
		}
		unstable {
			echo 'I am unstable :/'
		}
		failure {
			echo 'I failed :('

			office365ConnectorSend (
				status: "${currentBuild.currentResult}",
				webhookUrl: "${MSTEAMS_HOOK}",
				color: 'ff0000',
				message: "Build Failure: ${JOB_NAME} - ${BUILD_DISPLAY_NAME} " 
						   + "<br>Branch - ${env.BRANCH_NAME} " 
						   + "<br>Last Commiter: ${gitInfo('name')} - ${gitInfo('email')}" 
						   + "<br>Last Commit Message: ${gitInfo('message')}"
						   + "<br>Pipeline duration: ${currentBuild.durationString}"
			)
		}
		changed {
			echo 'Things were different before... '
		}
	}
	
}

def gitInfo(infoType) {
	def result;
	switch (infoType) {
         case "email": result = sh(returnStdout: true, script: "git --no-pager show -s --format='%ae'").trim(); 
		 	   break;
         case "name": result = sh(returnStdout: true, script: "git --no-pager show -s --format='%an'").trim(); 
		 	   break;
		 case "message": result = sh(returnStdout: true, script: "git log -1 --pretty=%cI' - '%h' - '%B").trim(); 
		 	   break;
         default: result = "";
      }
    
	return result;	
}
