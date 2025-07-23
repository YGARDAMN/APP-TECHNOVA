pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        SONAR_PROJECT_KEY = 'TechNova'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
        DOCKER_HUB_REPO = 'ygardamn004/technova'
    }
    stages {
        stage('GitHub') {
            steps {
                git branch: 'main', credentialsId: 'Jenkins', url: 'https://github.com/YGARDAMN/App-TechNova.git'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SonarQube', variable: 'SONAR_TOKEN')]) {
                    // Ganti 'SonarQube' dengan nama instance SonarQube kamu di Jenkins
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://sonarqube:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
        stage{'Docker Image'}{
            steps {
                script {
                    docker.build("${DOCKER_HUB_REPO}:latest")
                }
            }
        }
    }
}