pipeline{
	agent any
	environment{
		imagename = "vaibhavmangla00/currency-exchange-devops"
		registryCredential = 'dockerhub'
		dockerImage = ''
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
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
		stage('Package'){
			steps{
				sh 'mvn package -DskipTests '
			}
		}
		stage('Building image') {
			steps{
				script {
					dockerImage = docker.build imagename
					}
				}
			}
		stage('Deploy Image') {
			steps{
				script {
					docker.withRegistry( '', registryCredential ) {
					dockerImage.push("$BUILD_NUMBER")
					dockerImage.push('latest')
					}
				}
			}
		}
		stage('Remove Unused docker image') {
			steps{
				sh "docker rmi $imagename:$BUILD_NUMBER"
				sh "docker rmi $imagename:latest"
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