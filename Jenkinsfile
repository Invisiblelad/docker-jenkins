pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Invisiblelad/docker-jenkins.git']])
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerpwd')]) {
                    sh "docker login -u ${dockeruser} -p ${dockerpwd}"
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t ${dockeruser}/devops2 ."
            }
        }
        stage('Docker Push') {
            steps {
                sh "docker push ${dockeruser}/devops2"
            }
        }
        stage('Kubernetes Deploy') {
            steps {
                withKubeConfig([credentialsId: 'config']) {
                    sh """
                    kubectl apply -f deploy-service.yaml
                    """
                }
            }
        }
    }
}

