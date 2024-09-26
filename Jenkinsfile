pipeline {
    agent any
    stages {
        stage('Cleanning up Docker and Images'){
            steps{
                script{
                    // Check if any containers are running
                    def runningContainers = sh(script: 'docker ps -q', returnStatus: true) == 0

                    if (runningContainers) {
                        echo 'Stopping and removing running containers...'
                        sh 'docker stop $(docker ps -q) || true'
                        sh 'docker rm $(docker ps -aq) || true'
                    } else {
                        echo 'No running containers found.'

                        // Remove stopped containers
                        sh 'docker rm $(docker ps -aq) || true'
                    }

                    // Remove images
                    echo 'Removing images...'
                    sh 'docker rmi $(docker images -q) || true'

                    // Display message if no containers or images are present
                    def noContainers = sh(script: 'docker ps -q', returnStatus: true) != 0
                    def noImages = sh(script: 'docker images -q', returnStatus: true) != 0

                    if (noContainers && noImages) {
                        echo 'No containers and images found.'
                    }
                }
            }
        }
        stage('clone'){
            steps{
                git branch: 'dev', url: 'https://github.com/chandrasekhar552002/surya.git'
            }
        }
        stage('Build Image'){
            steps{
                script{
                    sh 'docker build -t myapp .'
                }
            }
        }
        stage('Running Container'){
            steps{
                script{
                    sh 'docker run --rm -itd --name cont_myapp -p 80:8080 myapp'
                }
            }
        }
    }
}