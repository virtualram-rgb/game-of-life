pipeline {
	agent any
	stages {
		stage('git') {
			steps {
				git branch: 'master', url: 'https://github.com/wakaleo/game-of-life.git'
			}
		}
		stage('build') {
			steps {
				mvn clean package
			}
		}
		stage('junit') {
			steps {
				junit '**/surefire-reports/*.xml'
			}
		}
	}
}