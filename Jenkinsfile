pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                'echo "Running Test Setup"'
                sh 'mvn clean test'
            }
        }
        stage('Build') {
            steps {
                sh '''
                echo "Running Build Setup"
                mvn clean install
                mkdir -p /home/jenkins/project-wars
                mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                echo "Running deployment"
                java -jar /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
    }
}