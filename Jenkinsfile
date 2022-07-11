pipeline {
    agent {label "java_build_node"}
    environment {
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
                sh 'docker push shaikhsharukh/nodejsapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
            sh 'echo "post build"'
        }
    }
}
