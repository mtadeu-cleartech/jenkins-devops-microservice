pipeline {

	//agent { docker { image 'maven:3.6.3'} }
	//agent any

	environment {
		dockerHome = tool 'dockerDefault'
		mavenHome = tool 'mavenDefault'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build "
				echo "PATH - $PATH"
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
