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
                buildNum=${BUILD_NUMBER}
                echo '[Unit]
Description=My SpringBoot App

[Service]
User=jenkins
Type=simple

ExecStart=/usr/bin/java -jar /home/jenkins/project-wars/project-'$buildNum'.war

[Install]
WantedBy=multi-user.target' > /home/jenkins/myApp.service
                sudo mv /home/jenkins/myApp.service /etc/systemd/system/myApp.service

                sudo systemctl daemon-reload    
                sudo systemctl restart myApp

                '''
            }
        }
    }
}