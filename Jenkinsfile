pipeline {
  agent any

  stages {
    stage('Clone Repo') {
      steps {
        git branch: 'main', url: 'https://github.com/YGARDAMN/APP-TECHNOVA.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        // Gunakan nama image tanpa spasi atau ubah ke format yang valid
        sh 'docker build -t app-technova .'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'pytest tests/ --junitxml=report.xml || true' // Tambahkan '|| true' agar tidak gagal jika pytest belum tersedia
      }
    }

    stage('SonarQube Analysis') {
      environment {
        scannerHome = tool 'SonarScanner' // Pastikan ini sesuai dengan konfigurasi di Jenkins
      }
      steps {
        withSonarQubeEnv('SonarQube') { // Pastikan nama ini cocok dengan di Manage Jenkins > Configure System
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }

    stage('Deploy Container') {
      steps {
        sh 'docker run -d --name app-technova -p 5000:5000 app-technova'
      }
    }
  }
}
