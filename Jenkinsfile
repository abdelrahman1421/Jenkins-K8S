pipeline {
    agent {label 'slave'}

    stages {
        stage('Build Docker Image') {
            steps {
                script{
                if (env.BRANCH_NAME == "master") {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {

                    sh """
                     docker build -f Dockerfile . -t engboda/bakehouse:${BUILD_NUMBER}
                     docker login -u ${USERNAME} -p ${PASSWORD}
                     docker push engboda/bakehouse:${BUILD_NUMBER}
                     echo ${BUILD_NUMBER} > ../vars.txt 
                     """ 
                    }
                }
              } 
            }
        }
        

        stage('Deploy') {
            steps {
                script {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'FILE')]) {
                if (env.BRANCH_NAME == "dev" || env.BRANCH_NAME == "test" || env.BRANCH_NAME == "prod") {
                    sh """
                     export NUMBER=\$(cat ../vars.txt)
                     cp Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                     cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                     rm -f Deployment/deploy.yaml.tmp
                     kubectl apply -f Deployment/deploy.yaml --kubeconfig=${FILE}
                     kubectl apply -f Deployment/service.yaml --kubeconfig=${FILE}
                     """
                    }
                }
              }
            }
        }
    }
}

