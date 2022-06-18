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
		stage('Package'){
			steps{
				sh 'mvn package -DskipTests '
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
				sh "exit"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]){
                    /**
                    * Restart docker server
                    **/
                    sh '''
                        echo "${password} | docker login -u ${username} --password-stdin"
                        docker stop docker_image
                        docker rm docker_image
                        docker pull docker_image:latest
                        docker run -d -p 8000:8000 --name docker-image-name -t docker_image:latest
                    '''
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