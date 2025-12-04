pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

    environment {
        SONARQUBE = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SARRA1920/devops.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar -Dsonar.projectKey=MonProjet -Dsonar.host.url=http://192.168.117.128:9000 -Dsonar.login=<sqa_aa6c19f1b5db65c2efca9b4578f69713c19f6618>'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
