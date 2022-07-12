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
                sh 'docker build -t sharukh/nodeapp:$BUILD_NUMBER . --no-cache'
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
                sh 'docker push shaikhsharukh/nodejsapp:$BUILD_NUMBER'
                sh 'docker rmi -f  $(docker images -q)'
            }
        }
}
post {
        failure {
            sh 'echo "sending email"'
        }
        success { 
            sh 'echo "deploying"'
            sh 'docker pull shaikhsharukh/nodejsapp:$BUILD_NUMBER'
            sh 'docker run -d -p 80:3000 --name new-node shaikhsharukh/nodejsapp:$BUILD_NUMBER /bin/bash' 
        }
        always {
            sh 'docker logout'
        }
    }
}
