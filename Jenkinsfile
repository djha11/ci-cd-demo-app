pipeline {
    agent any

    triggers {
        githubPush()     // This triggers auto build when code is pushed
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/djha11/ci-cd-demo-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Stop Old App') {
            steps {
                sh '''
                PID=$(netstat -ano | findstr :8081 | awk "{print \$5}")
                if [ ! -z "$PID" ]; then
                    taskkill /PID $PID /F
                fi
                '''
            }
        }

        stage('Deploy New App') {
            steps {
                sh "java -jar target/demo-1.0.0.jar &"
            }
        }
    }
}