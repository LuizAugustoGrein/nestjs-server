pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('install') {
            agent {
                docker { image 'node:20' }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('build') {
            agent {
                docker { image 'node:20' }
            }
            steps {
                sh 'npm run build'
            }
        }
        stage('test') {
            agent {
                docker { image 'node:20' }
            }
            steps {
                sh 'npm run test'
            }
        }
        stage('build image') {
            steps {
                sh 'docker build -t nestjs-server:1.0 .'
            }
        }
        stage('docker push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh '''
                        docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD
                        docker tag nestjs-server:1.0 luizaugustogreinunc/nestjs-server:1.0
                        docker push luizaugustogreinunc/nestjs-server:1.0
                        docker logout
                    '''
                }
            }
        }
    }
}
