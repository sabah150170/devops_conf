pipeline {
    agent any

    environment {
        // Define environment variables
	GITHUB_CREDENTIAL = credentials('GITHUB_CREDENTIAL')
        DOCKER_HUB_CREDENTIAL = credentials('DOCKER_HUB_CREDENTIAL')
        DOCKER_IMAGE_NAME_SERVER = 'bnsdcr/nodejs_server'
	DOCKER_IMAGE_NAME_DB = 'bnsdcr/postgresql_db'
	DOCKER_FILE_SERVER = 'Dockerfile_dev'
	DOCKER_FILE_DB = 'Dockerfile_db'
    }

    stages {
        stage('Checkout the code from Git') {
            steps {
                git branch: 'main', credentialsId: '${GITHUB_CREDENTIAL}', url: 'https://github.com/sabah150170/devops_deneme.git'
            }
        }

        stage('Build') {
            steps {
                // Build the Docker image
                script {
			dockerImage_1 = docker.build("${DOCKER_IMAGE_NAME_SERVER}:1", "--file ${DOCKER_FILE_SERVER}")
                    dockerImage_2 = docker.build("${DOCKER_IMAGE_NAME_DB}:1", "--file ${DOCKER_FILE_DB}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Log in to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIAL}") {
                        // Push the Docker image to Docker Hub
                        dockerImage1.push("1")
			dockerImage2.push("1")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Add your deployment steps here
                echo 'Deployment steps go here'
            }
        }
    }

    post {
        success {
            // Actions to be performed if the build is successful
            echo 'Build and deployment successful!'
        }
        failure {
            // Actions to be performed if the build fails
            echo 'Build or deployment failed!'
        }
    }
}