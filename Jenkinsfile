pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -f Dockerfile . -t engboda/bakehouse:$BUILD_NUMBER'
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                sh 'docker push engboda/bakehouse:${BUILD_NUMBER}'
                sh "echo ${BUILD_NUMBER} > ../vars.txt"  
            }
            }
        }
        stage('Deploy') {
            steps {
                 withCredentials([file(credentialsId: 'kubeconfig', variable: 'FILE')]) {
                sh 'export BUILD_NUMBER=\$(cat ../vars.txt)'
                sh 'kubectl apply -f Deployment/deploy.yaml --kubeconfig=${FILE}'
                sh 'kubectl apply -f Deployment/service.yaml --kubeconfig=${FILE}'
            }
            }
        }
    }
}
