pipeline {
    agent any

    triggers {
        githubPush()     // Auto build when code is pushed
    }

    stages {

        stage('Kill Old Spring Boot Process') {
            steps {
                bat '''
echo Checking for any process using port 8081...
for /F "tokens=5" %a in ('netstat -ano | findstr :8081') do (
  echo Killing process with PID %a
  taskkill /PID %a /F
) || echo No process found on port 8081.
exit 0
'''

            }
        }
	stage('Clean Workspace') {
    steps {
        cleanWs()  // Deletes all files in workspace before fetching code
    }
}

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/djha11/ci-cd-demo-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Stop Old Instance') {
            steps {
                bat '''
                @echo off
                echo Checking for existing process on port 8081...
                for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8081') do (
                    echo Killing process %%a on port 8081...
                    taskkill /PID %%a /F
                )
                echo Port 8081 is now free.
                '''
            }
        }

        stage('Deploy New App') {
            steps {
                bat '''
                echo Starting Spring Boot app on port 8081...
                powershell -Command "Start-Process java -ArgumentList '-jar target\\demo-1.0.0.jar --server.port=8081' -NoNewWindow"
                '''
            }
        }
    }
}
