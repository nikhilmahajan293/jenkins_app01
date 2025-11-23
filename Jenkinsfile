pipeline {
    // select any agent available
    agent any
    
    // list of environment variables
    environment {
        PATH = "${env.PATH};C:\\Program Files\\Docker\\Docker\\resources\\bin"
        
        // read the token from global variables section
        DOCKER_HUB_TOKEN = credentials('DOCKER_HUB_TOKEN')
        
        // GitHub repository
        GIT_REPO = "https://github.com/nikhilmahajan293/jenkins_app01.git"
        
        // image name
        DOCKER_IMAGE_NAME = "nikhilmahajan293/myapp"
    }

    stages {
        // scm stage to pull the code from GitHub repo
        stage ('scm') {
            steps {
                // pull the code from git repo on main branch
                echo "Pulling the code from GitHub repo"
                git branch: 'main', url: "${GIT_REPO}"
            }
        }
        
        // build the code
        stage('building code') {
            steps {
                // build the code
                echo "building the code"
            }
        }
        
        // test the code
        stage('testing app') {
            steps {
                echo "testing the application"
            }
        }
        
        // build the docker image
        stage('build docker image') {
            steps {
                bat """
                docker image build -t ${DOCKER_IMAGE_NAME} .
                """
            }
        }
        
        // login to the docker hub
        stage('docker hub login') {
            steps {
                bat """
                echo ${DOCKER_HUB_TOKEN} | docker login -u nikhilmahajan293 --password-stdin
                """
            }
        }
        
        // push the image to docker hub
        stage('push the image to dockerhub') {
            steps {
                bat """
                docker image push ${DOCKER_IMAGE_NAME}
                """
            }
        }
        
        // update the docker service
        stage('updating docker service') {
            steps {
                bat """
                docker service update --force --image ${DOCKER_IMAGE_NAME} myapp
                """
            }
        }
    }
}