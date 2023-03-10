pipeline {
    agent any

    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
    
    stages {

        stage('SCM checkout') {
            steps {
                echo 'checkout starts'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/EbYVarghese18/myapp-nginx']])
                echo 'checkout ends'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Build Dockerimage starts'
                script{
                    sh 'docker build -t ebinvarghese/myapp-nginx:${TAG} .'
                }
                echo 'Build Dockerimage ends'
            }
        }
        
        stage('Push Docker image to Dockerhub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerpwd')]) {
                    sh 'docker login -u ebinvarghese -p ${dockerpwd}'
                    }
                    sh 'docker tag ebinvarghese/myapp-nginx:${TAG} ebinvarghese/myapp-nginx:latest'
                    sh 'docker push ebinvarghese/myapp-nginx:${TAG}'
                    sh 'docker push ebinvarghese/myapp-nginx:latest'
                    sh 'docker logout'
                }
            }
        }
        
    }
}   