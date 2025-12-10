pipeline {
    agent any

    tools {
        maven 'Maven 3.9.2'
        jdk 'JDK 17'
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
                    sh 'echo JAVA_HOME=$JAVA_HOME'
                    sh 'java -version'
                    sh 'echo M2_HOME=$M2_HOME'
                    sh 'mvn -version'
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('timesheet-devops') {
                    echo "🔍 Analyse SonarQube"
                    withSonarQubeEnv('sonarqube') {
                        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                            // Single quotes + env.VARIABLE pour sécuriser le token
                            sh '''
                                mvn sonar:sonar \
                                  -Dsonar.projectKey=MonProjet \
                                  -Dsonar.host.url=http://localhost:9000 \
                                  -Dsonar.login=$SONAR_TOKEN
                            '''
                        }
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "⏳ Vérification du Quality Gate"
                timeout(time: 10, unit: 'MINUTES') {
                    // Récupération du Quality Gate
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
