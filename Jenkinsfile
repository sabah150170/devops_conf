pipeline {
    agent any
    
    environment {
    // Define environment variables
        DOCKER_HUB_CREDENTIALS = "dockerhub"
        DOCKER_IMAGE_NAME_APP = "bnsdcr/nodejs_app"
    	DOCKER_IMAGE_NAME_DB = "bnsdcr/postgresql_db"
    	DOCKERFILE_APP = "Dockerfile_App"
    	DOCKERFILE_DB = "Dockerfile_DB"
    	DOCKER_IMAGE_TAG = "latest"
    	dockerImage1 = ''
    }
    
    stages {
        stage("Checkout the code from Git") {
            steps {
                git credentialsId: 'github', url: 'https://github.com/sabah150170/devops_deneme.git', branch: 'master'
            }
        }
        
    
        stage('Build the Docker images') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME_APP} -f ${DOCKERFILE_APP} . "
                }
            }
        }
        
        stage('Login and Push the Docker images') {
            steps {
                script {
                    docker.withRegistry( '', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE_NAME_APP}").push()

                    }
                }
            }
        }
        
    }
}
