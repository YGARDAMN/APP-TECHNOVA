pipeline {
    agent none
    stages {
        stage("Build & SonarQube Analysis") {
            agent any
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}
