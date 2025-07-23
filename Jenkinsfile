pipeline {
    agent any
    tools{
        nodejs 'NodeJS'
    }
    environment {
        SONAR_PROJECT_KEY = 'TechNova'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }
    stages {
        stage('GitHub'){
            steps {
                git branch: 'main', credentialsId: 'Jenkins', url: 'https://github.com/YGARDAMN/App-TechNova.git'
            }
        }
        stage('Unit Test'){
            steps {
                sh 'npm test'
                sh 'npm install'
            }
        }
        stage('SonarQube Analysis'){
            steps {
                withCredentials([string(credentialsId: 'SonarQube', variable: 'SONAR_TOKEN')]) {
                        
                        withSonarQubeEnv(credentialsId: 'SonarQube') {
                                sh """
                                ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                                -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://localhost:9000 \
                                -Dsonar.login=${SONAR_TOKEN}
                                """
                    }
                }
            }
        }
    }
}