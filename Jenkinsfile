 pipeline {
		agent {label 'docker'}

	environment {
		AWS_ACCOUNT_ID=" 250808210050"
		AWS_DEFAULT_REGION="us-east-2"
		IMAGE_REPO_NAME="docker"
		IMAGE_TAG="2"
		REPOSITORY_URI = "250808210050.dkr.ecr.us-east-2.amazonaws.com/docker"
	}

	stages {

		stage('Git checkout') {
			steps {
				git 'https://github.com/Nithishkumar0064/new-javafile.git'
			}
		}

	// Building Docker images
	stage('Docker build') {
		steps{
			script {
				sh "docker build -t ${REPOSITORY_URI}:${IMAGE_TAG} ."
			}
		}

	}
	stage('Logging into AWS ECR') {
		steps {
			script {
				sh """aws ecr get-login-password \
					--region ${AWS_DEFAULT_REGION} \
						| docker login \
						--username AWS \
						--password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
			}

		}
	}

// Uploading Docker images into AWS ECR
	stage('Pushing to ECR') {
		steps{
			script {
				sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
					}
			}
	}

		stage ('deploy to docker server'){
			steps{
				script{
					sh "docker run -d -p 9000:8080 ${REPOSITORY_URI}:${IMAGE_TAG}"
				}
			}
		}

	}
 }
