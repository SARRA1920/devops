pipeline {
    agent any

    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
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
                        // Variante plus explicite avec token (si besoin, facultatif) :
                        /*
                        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                            sh """
                                mvn clean verify -DskipTests sonar:sonar \
                                  -Dsonar.projectKey=MonProjet \
                                  -Dsonar.host.url=${SONAR_HOST_URL} \
                                  -Dsonar.token=$SONAR_TOKEN
                            """
                        }
                        */
                    }
                }
            }
        }

        // OPTION 2 : garder le Quality Gate, mais UNIQUEMENT si la config Jenkins/SonarQube est OK
        stage('Quality Gate') {
            steps {
                echo "⏳ Vérification du Quality Gate"
                timeout(time: 10, unit: 'MINUTES') {
                    // Récupération du Quality Gate
                    // ATTENTION : ceci nécessite que le serveur 'sonarqube' dans Jenkins ait un token valide
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