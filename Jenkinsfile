pipeline {
    agent any
//    stages {
//        stage('Preparations') {
//            steps {
//                script {
//                    echo "Preparing!"
//                }
//            }
//        }
       stage('Test') {
            steps {
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
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
                     def remote = [name: 'test', host: 'test.test.com', user: 'rao', password: "password123', allowAnyHosts: true]
                     sshCommand remote: remote, command: "for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done"
                 }
            }
        }
    }
}
