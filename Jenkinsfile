pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java17'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/imen-attia/td.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') { // Make sure 'sonarqube' matches your Jenkins SonarQube config
                    sh 'mvn sonar:sonar -Dsonar.projectKey=ecommerce'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                sudo cp target/*.war /var/lib/tomcat9/webapps/ecommerce.war
                sudo systemctl restart tomcat9
                sleep 10
                '''
            }
        }

        stage('Check Application') {
            steps {
                sh 'curl -f http://192.168.132.160:8081/ecommerce'
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                sh 'zap-baseline.py -t http://192.168.132.160:8081/ecommerce || true'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.war', allowEmptyArchive: false
        }
    }
}
