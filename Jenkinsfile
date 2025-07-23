pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YGARDAMN/APP-TECHNOVA.git'
            }
        }

        stage('Build & SonarQube') {
            steps {
                dir('backend') { // ganti sesuai folder pom.xml
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
