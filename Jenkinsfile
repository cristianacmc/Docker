pipeline {
    agent any
        environment {
        DB_PASSWORD = 'DB_PASSWORD'
    }
    stages {
        stage('Clone Directory') {
            steps {
                git branch: 'main', url: 'https://github.com/cristianacmc/Docker.git'
            }
        }
        stage('Install Docker') {
            steps {
                sh '''sh "sudo apt-get update && sudo apt install curl -y"
                sh "curl https://get.docker.com | sudo bash"
                sh \'sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                sh "sudo chmod +x /usr/local/bin/docker-compose"'''
            }
        }
        stage('Deploy') {
            steps {
                sh 'sh \'sudo docker-compose -f chaperootodo_client/docker-compose.yaml pull && sudo -E DB_PASSWORD=${DB_PASSWORD} docker-compose -f chaperootodo_client/docker-compose.yaml up -d\''
                
            }
        }
    }
}