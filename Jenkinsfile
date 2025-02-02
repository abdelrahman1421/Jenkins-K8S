pipeline {
    agent any

    stages {
        stage('Pull Web Server Image') {
            steps {
                script {
                    echo "Pulling Nginx image..."
                    sh 'docker pull nginx'
                }
            }
        }

        stage('Run Web Server Container') {
            steps {
                script {
                    echo "Running Nginx container..."
                    sh 'docker run -d --name webserver -p 50:80 nginx'
                }
            }
        }

        stage('Check Running Containers') {
            steps {
                script {
                    echo "Listing running containers..."
                    sh 'docker ps'
                }
            }
        }
    }
}
