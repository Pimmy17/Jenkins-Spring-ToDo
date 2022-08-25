pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                echo "Running Maven Clean and Test Command"
                sh 'mvn clean test'
            }
        }
        stage('Build') {
            steps {
                echo "Running Build Command"
                sh '''
                mvn clean install
                mkdir -p /home/jenkins/project-wars
                mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo "Running Deployment"
                sh '''
                java -jar /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
    }
}