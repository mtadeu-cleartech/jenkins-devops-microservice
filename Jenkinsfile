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

		stage('Deploy QA') {

			when {
                expression { BRANCH_NAME ==~ /(develop|release)/ }
            }
        	        
            steps {
                echo "aguardar input"
                input message: 'Efetuar Deploy para QA? (Click "Sim" para continuar)'
                echo "Proceed true"
            }
        }

		stage('Deploy Homologação') {

			when {
                branch 'release'
            }
        	        
            steps {
                echo "aguardar input"
                input message: 'Efetuar Deploy para Homologação? (Click "Sim" para continuar)'
                echo "Proceed true"
            }
        }
		
		stage('Deploy Produção') {
	
			when {
                branch 'release'
            }

            steps {
                echo "aguardar input"
                input message: 'Efetuar Deploy para Produção? (Click "Sim" para continuar)'
                echo "Proceed true"
            }
        }

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
