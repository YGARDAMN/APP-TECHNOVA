pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube'
        DOCKER_IMAGE = 'ygardamn004/technova-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YGARDAMN/APP-TECHNOVA.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'pytest tests/unit --junitxml=unit-test-report.xml'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'pytest tests/integration --junitxml=integration-test-report.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'echo "Simulasi deploy ke staging dengan Docker Compose atau K8s"'
            }
        }

        stage('Slack Notification') {
            steps {
                slackSend(channel: '#ci-cd', message: "Pipeline selesai ✅ - Build sukses", color: '#36a64f')
            }
        }
    }

    post {
        failure {
            slackSend(channel: '#ci-cd', message: "❌ Pipeline gagal: ${env.BUILD_URL}", color: '#ff0000')
        }
    }
}
