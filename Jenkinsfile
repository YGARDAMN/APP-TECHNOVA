pipeline {
	agent any
	tools{
		nodejs 'NodeJS'
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
	}

}