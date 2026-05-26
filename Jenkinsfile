pipeline {
    agent any
    
    //Thông báo
    def sendNotification(String stageName, String status) {
        def color = (status == 'SUCCESS') ? 'good' : 'danger'
        slackSend(
            color: color,
            message: "Stage [${stageName}]: ${status} - Build #${env.BUILD_NUMBER} (${env.BUILD_URL})"
        )
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
            post {
                success { sendNotification('Checkout', 'SUCCESS') }
                failure { sendNotification('Checkout', 'FAILURE') }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
            post {
                success { sendNotification('Install', 'SUCCESS') }
                failure { sendNotification('Install', 'FAILURE') }
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
            post {
                success { sendNotification('Test', 'SUCCESS') }
                failure { sendNotification('Test', 'FAILURE') }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t express-hello-world:latest .'
            }
            post {
                success { sendNotification('Build Docker', 'SUCCESS') }
                failure { sendNotification('Build Docker', 'FAILURE') }
            }
        }
    }
}