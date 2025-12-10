pipeline {
    agent any
    
    tools {
        maven 'M2_HOME'  // ✅ Utilisez le nom que vous avez configuré dans Jenkins
        jdk 'JAVA_HOME'  // ✅ Utilisez le nom que vous avez configuré dans Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "🔄 Checkout du code depuis Git"
                git branch: 'main', url: 'https://github.com/SARRA1920/devops.git'
            }
        }
        
        stage('Build') {
            steps {
                dir('timesheet-devops') {
                    echo "🛠️ Compilation du projet avec Maven"
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                dir('timesheet-devops') {
                    echo "🔍 Analyse SonarQube"
                    withSonarQubeEnv('SonarQube') {  // ✅ Utilisez le nom exact de votre serveur SonarQube dans Jenkins
                        sh 'mvn sonar:sonar -Dsonar.projectKey=MonProjet'
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                echo "⏳ Vérification du Quality Gate"
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}