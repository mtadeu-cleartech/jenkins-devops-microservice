pipeline {

	//agent { docker { image 'maven:3.6.3'} }
	agent any

	environment {
		dockerHome = tool 'dockerDefault'
		mavenHome = tool 'mavenDefault'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
		PROJECT_ROOT = "."
		POM_PATH = "${PROJECT_ROOT}/pom.xml"
		
		def pom = readMavenPom file:POM_PATH
		
	    ARTIFACT_ID = pom.getArtifactId()
	    VERSION = pom.getVersion()
	    GROUP_ID = pom.getGroupId()
	}
	

	stages {

		stage('Read-JSON') {
			steps {
				script {
					def oldJson = '{"branch":{"type-0.2":{"version":"0.2","rc":"1","rel":"1","extras":"1"}}}'
					def props = readJSON text: oldJson
					println(props['branch']['type-0.2']['rc'])

					echo "props version: ${props['branch']['type-0.2']['version']}"

					def packageJson = readJSON file: './package.json'
					echo "props version2: ${packageJson['version']}"
				}
			}
		}



		stage('Check Environment') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build "
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
				echo "POM_PATH - $POM_PATH"
				echo "ARTIFACT_ID - $ARTIFACT_ID"
				echo "VERSION - $VERSION"
				echo "GROUP_ID - $GROUP_ID"
				echo "BRANCH_NAME - $env.BRANCH_NAME"

				
			}
			
		}

		/*
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}

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
			echo 'I succeeeded!'
		}
		unstable {
			echo 'I am unstable :/'
		}
		failure {
			echo 'I failed :('
		}
		changed {
			echo 'Things were different before... '
		}
	}
	
}
