pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        REMOTE_IP = '172.17.0.1'
        REMOTE_PORT = '2222'
    }

    stages {
        stage('Checkout'){
            steps {
                git branch: 'main', url: 'https://github.com/AkhsanDaffa/my-cicd-portfolio.git'
            }
        }

        stage('Install & Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy Remote SSH') {
            steps {
                sshagent(['ssh-deploy-key']){
                    sh "ssh -o StrictHostKeyChecking=no -p ${REMOTE_PORT} root@${REMOTE_IP} 'rm -rf /var/www/html/*'"

                    sh "scp -o StrictHostKeyChecking=no -P ${REMOTE_PORT} -r dist/* root@${REMOTE_IP}:/var/www/html/"

                    sh "ssh -o StrictHostKeyChecking=no -p ${REMOTE_PORT} root@${REMOTE_IP} 'service nginx reload'"
                }
            }
        }
    }
}