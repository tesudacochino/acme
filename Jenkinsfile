pipeline {
    agent any
    stages {
       stage('Test') {
            steps {
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
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
                echo "end"
                sshagent(credentials : ['use-the-id-from-credential-generated-by-jenkins']) {
                    sh 'ssh -o StrictHostKeyChecking=no user@hostname.com uptime'
                    sh 'ssh -v user@hostname.com'
                    sh 'scp ./source/filename user@hostname.com:/remotehost/target'
                }
            }
        }
    }
}
