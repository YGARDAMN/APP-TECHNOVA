pipeline {
  agent any
  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/YGARDAMN/APP-TECHNOVA'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t LENOVO IDEAPAD/APP-TECHNOVA .'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'pytest tests/ --junitxml=report.xml'
      }
    }
    stage('SonarQube Analysis') {
      environment {
        scannerHome = tool 'SonarScanner'
      }
      steps {
        withSonarQubeEnv('SonarQubeServer') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
    stage('Deploy Container') {
      steps {
        sh 'docker run -d --name APP-TECHNOVA -p 5000:5000 LENOVO IDEAPAD/APP-TECHNOVA'
      }
    }
  }
}
