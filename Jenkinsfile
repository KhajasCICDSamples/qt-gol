pipeline {
    agent { label 'ltelog'}
    triggers {
        cron('59 23 * * *')
    }
    stages {
        stage('scm') {
            steps {
                git 'https://github.com/KhajasCICDSamples/qt-gol.git'        
            }
        }
        stage('build') {
            steps {
                sh script: 'mvn clean package'
            }
        }
        stage('post build') {
            steps {
                junit 'gameoflife-web/target/surefire-reports/*.xml'
                archiveArtifacts 'gameoflife-web/target/*.war'
            }
        }
    }
}