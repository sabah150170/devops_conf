pipeline {
    agent any
    
    environment {
    // Define environment variables
        DOCKER_HUB_CREDENTIALS = "dockerhub"
        DOCKER_IMAGE_NAME_APP = "bnsdcr/nodejs_app"
    	DOCKER_IMAGE_NAME_DB = "bnsdcr/postgresql_db"
    	DOCKERFILE_APP = "Dockerfile_App"
    	DOCKERFILE_DB = "Dockerfile_DB"
        MANIFEST_FILE = "kubernetes/app-deployment.yaml"
    	DOCKER_IMAGE_TAG = "latest"
    	dockerImage1 = ''
    }
    /*
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
                    sh "docker build -t ${DOCKER_IMAGE_NAME_DB} -f ${DOCKERFILE_DB} . "
                }
            }
        }
        
        stage('Login and Push the Docker images') {
            steps {
                script {
                    docker.withRegistry( '', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE_NAME_APP}").push("${env.BUILD_NUMBER}")
                        docker.image("${DOCKER_IMAGE_NAME_DB}").push()
                    }
                }
            }
        }
        */

        stage("Checkout Manifest code from Git") {
            steps {
                git credentialsId: 'github', url: 'https://github.com/sabah150170/devops_conf.git', branch: 'main'
            }
        }

        stage('Update Manifest') {
            //build job: 'updatemanifest' parameters: [string(name: 'DOCKER_TAG', value: env.BUILD_NUMBER)]
            steps {
                script {
                  catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email busenur.sabah@hotmail.com"
                        sh "git config user.name sabah150170"
                        sh "sed -i 's+bnsdcr/nodejs_app.*+bnsdcr/nodejs_app:${env.BUILD_NUMBER}+g' '${MANIFEST_FILE}'"
                        sh "git add ."
                        sh "git commit -m 'update manifest'"
                        sh "git push https://${GIT_USERNAME}:${encodedPassword}@github.com/${GIT_USERNAME}/devops_conf.git"
                    }
                  }
                }
            }
        }
    }
}
