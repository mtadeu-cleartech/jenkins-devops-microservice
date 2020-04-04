pipeline {

	agent { docker { image 'maven:3.6.3'} }
	//agent any

	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				echo "Build in Docker"
			}
			
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Test"
			}
		}
	}
	
}
