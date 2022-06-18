pipeline{
	agent any
	environment{
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		registryCredentials = 'dockerhub'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'docker version'
			}	
		}

		stage('Compile'){
			steps{
				sh 'mvn clean compile'
			}
		}
		stage('Test'){
			steps{
				sh 'mvn test'
			}
		}
		stage('Integration Test'){
			steps{
				sh 'mvn failsafe:integration-test failsafe:verify'
			}
		}
		stage('Build Docker Image'){
			steps{
				//"docker build -t vaibhavmangla00/currency-exchange-devops:$env.BUILD_TAG"
				script{
					dockerImage = docker.build("vaibhavmangla00/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('','dockerhub'){
						dockerImage.push()
						dockerImage.push('latest')
					}
					
				}
			}
		}
	}
	post{
		always{
			echo 'I run always'
		}
		success{
			echo 'I run on SUCCESS'
		}
		failure{
			echo 'I run when u FAIL'
		}
	}
}