pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git 'https://github.com/abdelrahman1421/Jenkins-K8S.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -f Dockerfile . -t engboda/bakehouse:$BUILD_NUMBER'
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                sh 'docker push engboda/bakehouse:$BUILD_NUMBER'
            }
            }
        }
        stage('Deploy') {
            steps {
                 withCredentials([file(credentialsId: 'kubeconfig', variable: 'FILE')]) {
                sh 'kubectl apply -f Deployment/deploy.yaml --kubeconfig=${FILE}'
                sh 'kubectl apply -f Deployment/service.yaml --kubeconfig=${FILE}'
            }
            }
        }
    }
}
