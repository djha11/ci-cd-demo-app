pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', git 'https://github.com/djha11/ci-cd-demo-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Stop Old App (if running)') {
            steps {
                sh 'pkill -f demo-1.0.0.jar || true'
            }
        }

        stage('Deploy New App') {
            steps {
                sh 'nohup java -jar target/demo-1.0.0.jar > app.log 2>&1 &'
            }
        }
    }
}