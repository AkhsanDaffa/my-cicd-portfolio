pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Checkout'){
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Docker') {
            steps {
                script {
                    sh 'docker build -t my-react-app .'

                    sh 'docker stop portfolio-container || true'
                    sh 'docker rm portfolio-container || true'

                    sh 'docker run -d --name portfolio-container -p 3001:80 my-react-app'
                }
            }
        }
    }
}