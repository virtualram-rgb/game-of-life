pipeline {
    agent any
    stages {
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virtualram-rgb/game-of-life.git', branch: 'master'
            }
        }
        stage ('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage ('build') {
            steps {
                withSonarQubeEnv('sonar'){
                    sh 'mvn install sonar:sonar'
                }
            }
        }
        stage('junit') {
			steps {
				junit '**/surefire-reports/*.xml'
			}
		}
        stage('exec maven'){
            steps{
                rtmavenRun (
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                )
            }
        }
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER" ,
                    serverId: 'jfrog_instance',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )
            }
        }
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
    }
}
