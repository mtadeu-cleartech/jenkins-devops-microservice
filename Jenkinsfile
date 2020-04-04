pipeline {

<<<<<<< HEAD
	//agent { docker { image 'maven:3.6.3'} }
	//agent any

	environment {
		dockerHome = tool 'dockerDefault'
		mavenHome = tool 'mavenDefault'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
=======
	agent { docker { image 'maven:3.6.3'} }
	//agent any
>>>>>>> e30cc25deddc6a8152e704d8f97d346525194984

	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
<<<<<<< HEAD
				sh 'docker version'
				echo "Build "
				echo "PATH - $PATH"
=======
				echo "Build in Docker"
>>>>>>> e30cc25deddc6a8152e704d8f97d346525194984
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
