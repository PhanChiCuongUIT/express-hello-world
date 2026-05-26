//Thông báo
def sendNotification(String stageName, String status) {
    if (status == 'SUCCESS') {
        echo "✅ THÀNH CÔNG: Hoàn thành bước [${stageName}]"
    } else {
        echo "❌ THẤT BẠI: Lỗi tại bước [${stageName}]. Vui lòng kiểm tra lại!"
    }
}

pipeline {
    agent any

    tools {
        nodejs 'node18' 
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