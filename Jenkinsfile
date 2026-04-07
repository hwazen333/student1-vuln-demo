pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        SONAR_HOST_URL = 'http://192.168.119.129:9000'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Checkout') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Snyk Scan') {
            steps {
                echo 'Snyk scan stage'
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                    npx sonar-scanner \
                    -Dsonar.projectKey=student1-vuln-demo \
                    -Dsonar.projectName=student1-vuln-demo \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.token=$SONAR_TOKEN
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t student1-vuln-demo:latest .'
            }
        }

        stage('Run App') {
            steps {
                sh 'docker rm -f student1-app || true'
                sh 'docker run -d --name student1-app -p 3003:3000 student1-vuln-demo:latest'
            }
        }

        stage('ZAP Scan') {
            steps {
                echo 'ZAP scan stage'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                echo 'Kubernetes deploy stage'
            }
        }
    }
}
