pipeline {
    agent any
    stages {
        stage('code checkout') {
            steps {
                echo "cloning the code from github"
                git branch: 'main', url: 'https://github.com/gopalakrishna-1/django-notes-app.git'
            }
        }
        stage('code build') {
            steps {
                echo "building the image from docker"
                sh 'docker build -t node-app .'
            }
        }
        stage('push') {
            steps {
                echo "docker image pushing to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]) {
                    sh "docker tag node-app ${env.dockerhubuser}/node-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/node-app:latest"
                }
            }
        }
        stage('deploy') {
            steps {
                echo "deploy the image into ec2 instance"
                sh 'docker-compose down && docker-compose up -d'
                
            }
        }
    }
}
