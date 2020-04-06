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
		stage('Checkout') {
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
			}
			
		}

		/*
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}


		stage('Test') {
			steps {
				sh "mvn test"
			}
		}

		stage('Integration Test') {
			steps {
				//sh "mvn failsafe:integration-test failsafe:verify"
				echo "Integration test escaped"
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
					dockerImage = docker.build("matmedeiros/currency-exchange-devops:${env.BUILD_TAG}")
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
	
}
