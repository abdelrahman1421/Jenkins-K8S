pipeline {
    agent {label 'slave'}

    stages {
        stage('Build Docker Image') {
            steps {
                if (env.BRANCH_NAME == "master") {
                    sh 'docker build -f Dockerfile . -t engboda/bakehouse:$BUILD_NUMBER'
                    {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                    sh 'docker push engboda/bakehouse:${BUILD_NUMBER}'
                    sh "echo ${BUILD_NUMBER} > ../vars.txt"  
                    }
                }
            }
        }
        }

        stage('Deploy') {
            steps {
                if (env.BRANCH_NAME == "dev" || env.BRANCH_NAME == "test" || env.BRANCH_NAME == "prod")
                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'FILE')]) {
                    sh 'export BUILD_NUMBER=\$(cat ../vars.txt)'
                    sh 'cp Deployment/deploy.yaml Deployment/deploy.yaml.tmp'
                    sh 'cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml'
                    sh 'rm -f Deployment/deploy.yaml.tmp'
                    sh 'kubectl apply -f Deployment/deploy.yaml --kubeconfig=${FILE}'
                    sh 'kubectl apply -f Deployment/service.yaml --kubeconfig=${FILE}'
                    }
                }
            }
        }
    }
}
