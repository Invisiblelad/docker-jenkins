pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Invisiblelad/python_app.git']])
            }
        }
        stage('Docker Login'){
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                sh "docker login -u ${dockerusername} -p ${dockerhubpwd}"
                }
            }
        }
        stage('docker build'){
            steps {
                sh docker build -it ${dockerusername}/devops1 .
            }
        }
        stage('docker push'){
            steps {
                sh docker push  ${dockerusername}/devops1
            }
        }
}
}