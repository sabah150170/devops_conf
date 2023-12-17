pipeline {
    agent any
    
    environment {
    // Define environment variables
        DOCKER_HUB_CREDENTIALS = "dockerhub"
        DOCKER_IMAGE_NAME_APP = "bnsdcr/nodejs_app"
    	DOCKER_IMAGE_NAME_DB = "bnsdcr/postgresql_db"
    	DOCKER_PATH_APP = "DockerFileApp/"
    	DOCKER_PATH_DB = "DockerFileDB/"
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
                dir('${DOCKER_PATH_APP}'){
                    script{
                        dockerImage1 = docker.build("${DOCKER_IMAGE_NAME_APP}")
                    }
                }
                
                //dir('${DOCKER_PATH_DB}'){
                //    script{
                //        dockerImage2 = docker.build("${DOCKER_IMAGE_NAME_DB}")
                //    }
                //}
            }
        }
        
        stage('Login and Push the Docker images') {
            steps {
                script {
                    docker.withRegistry( '', DOCKER_HUB_CREDENTIALS) {
                        dockerImage1.push()
                    }
                }
            }
        }
        
    }
}
