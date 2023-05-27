pipeline {
	agent { label 'build_node' }
    stages {
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virtualram-rgb/game-of-life.git', branch: 'master'
            }
        }  
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER" ,
                    serverId: 'jfrog_instance',
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }
	stage ('build') {
            steps {
                withSonarQubeEnv('SONAR_SELF_HOSTED'){
                    sh 'mvn package sonar:sonar'
                }
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
