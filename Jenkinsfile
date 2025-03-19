pipeline {
    agent any
    environment {
        TOMCAT_URL = 'http://<your-ec2-public-ip>:8080/manager/text'
        TOMCAT_CREDS = credentials('tomcat-credentials')
        SONARQUBE_TOKEN = credentials('sonarqube-token')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/your-username/NumberGuessGame.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONARQUBE_TOKEN'
                }
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sh 'mvn tomcat7:deploy -Dmaven.tomcat.url=$TOMCAT_URL -Dmaven.tomcat.username=$TOMCAT_CREDS_USR -Dmaven.tomcat.password=$TOMCAT_CREDS_PSW'
            }
        }
    }
    post {
        success {
            mail to: 'your-email@example.com', subject: 'Pipeline Success', body: 'The pipeline completed successfully.'
        }
        failure {
            mail to: 'your-email@example.com', subject: 'Pipeline Failed', body: 'The pipeline failed.'
        }
    }
}
