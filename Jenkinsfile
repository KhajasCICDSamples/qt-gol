pipeline {
    agent { label 'ltecomm'}
    triggers {
        cron('H * * * 1-5')
    }
    stages {
        stage('scm') {
            steps {
                git branch: 'developer', url:'https://github.com/KhajasCICDSamples/qt-gol.git'        
            }
        }
        stage('build') {
            steps {
                withSonarQubeEnv('SONAR-7.1') {
                    sh script: 'mvn clean package sonar:sonar'

                }
                
            }
        }
        
        stage('post build') {
            steps {
                junit 'gameoflife-web/target/surefire-reports/*.xml'
                archiveArtifacts 'gameoflife-web/target/*.war'
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
    }
    post {
        always {
            mail to: 'learningthoughts.in@gmail.com', 
                subject: "Status of pipeline ${currentBuild.fullDisplayName}",
                body: "${env.BUILD_URL} has result ${currentBuild.result}"
        }
    }
}