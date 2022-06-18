pipeline{
	agent any
	environment{
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