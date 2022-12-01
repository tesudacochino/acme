pipeline {
    agent any
    stages {
        stage('Preparations') {
            steps {
                script {
                    echo "Preparing!"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo "Testing!"
                }
            }
        }
        stage('Build') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                    dockerImage = docker.build("acme-app:${env.BRANCH_NAME}-${env.BUILD_ID}")
                }
            }
        }
        stage('Push docker image') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                    docker.withRegistry('http://localhost:5000') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy docker') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                   
                    ssh -o StrictHostKeyChecking=no root@192.168.21.131 ${dockerRun}
                
                }
            }
        }
    }
}
