pipeline {
    agent any

    stages {
        stage('Pull Web Server Image') {
            steps {
                script {
                    echo "Pulling yeasy/simple-web image..."
                    sh 'docker pull yeasy/simple-web
'
                }
            }
        }

        stage('Run Web Server Container') {
            steps {
                script {
                    echo "Running Nginx container..."
                    sh 'docker run -d --name webserver -p 90:80 yeasy/simple-web
'
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
