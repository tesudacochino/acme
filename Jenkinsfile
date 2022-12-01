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
        stage('Deploy') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                    def dockerRun = 'docker run -p 8080:8080 -d --name my-app kammana/my-app:2.0.0'
                    sshagent(['dev-server']) {
                    sh "ssh -o StrictHostKeyChecking=no test@127.0.0.1 ${dockerRun}"
                    }
                }
            }
        }
    }
}
