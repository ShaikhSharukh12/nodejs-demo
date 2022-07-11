pipeline {
    agent {label "java_build_node"}
    environment {
    EMAIL_TO = 'realshad07@gmail.com'
    DOCKERHUB_CREDENTIALS = credentials('docker-hub-sharukh')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git 'https://github.com/ShaikhSharukh12/nodejs-demo.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t sharukh/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker tag sharukh/nodeapp:$BUILD_NUMBER shaikhsharukh/nodejsapp:$BUILD_NUMBER'
                sh 'docker push shaikhsharukh/nodejsapp:$BUILD_NUMBERvdfghf'
            }
        }
}
post {
        failure {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
        }
        success { 
            sh 'echo success'
        }
        always {
            sh 'docker logout'
        }
    }
}
