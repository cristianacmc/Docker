pipeline {
    agent any
        environment{
        DB_PASSWORD = credentials('DATABASE_PASSWORD')
    }
    stages {
        stage('Clone Directory') {
            steps {
                git branch: 'main', url: 'https://github.com/cristianacmc/Docker.git'
            }
        }
        stage('Install Docker') {
            steps {
            sh '''sh "sudo apt-get update
                  sudo apt install curl jq -y
                  curl https://get.docker.com | sudo bash"
                  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                  sudo chmod +x /usr/local/bin/docker-compose"'''
            }
        }
        stage('Deploy') 
            steps {
                with 
                sh '''docker-compose pull
                docker-compose up -d'''
            }
        }
    }
}