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
                ssh -i ./.ssh/id_rsa jenkins@18.132.114.79 << EOF
                git clone https://github.com/Pimmy17/Jenkins-Spring-ToDo.git
                cd Jenkins-Spring-ToDo
                git checkout main
                git pull
                mvn clean install
                mkdir -p /home/jenkins/project-wars
                mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                EOF
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo "Running Deployment"
                sh '''
                ssh -i ./.ssh/id_rsa jenkins@18.132.114.79 << EOF
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
                EOF
                '''
            }
        }
    }
}