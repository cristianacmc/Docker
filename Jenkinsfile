pipeline {
    agent any
        environment{
        DB_PASSWORD = credentials('DATABASE_PASSWORD')
    }
    stages {
        stage('Clone Directory') {
            steps {
                git branch: 'master', url: "https://gitlab.com/qacdevops/chaperootodo_client.git" 
            }
        }
        stage('Install Docker') {
            steps {
                sh "sudo apt-get update && sudo apt install curl -y"
                sh "curl https://get.docker.com | sudo bash"
                sh 'sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                sh "sudo chmod +x /usr/local/bin/docker-compose"
            }
        }
        stage('Deploy') {
            steps {
                sh "sudo docker-compose -f ./docker-compose.yaml pull && sudo -E DB_PASSWORD=${DB_PASSWORD} docker-compose -f ./docker-compose.yaml up -d"
            }
        }
    }
}