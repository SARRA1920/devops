pipeline {
    agent any
    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }
    environment {
        SONARQUBE = 'SonarQube'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SARRA1920/devops.git'  // ✅ Ajout de branch: 'main'
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
                    bat 'mvn sonar:sonar -Dsonar.projectKey=MonProjet -Dsonar.host.url=http://192.168.117.128:9000 -Dsonar.login=sqa_aa6c19f1b5db65c2efca9b4578f69713c19f6618'  // ✅ Retiré les < >
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