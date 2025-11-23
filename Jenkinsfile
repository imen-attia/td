pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/marwaiset/e-commerce.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'cp target/*.war /opt/tomcat/webapps/'
            }
        }
    }
}
