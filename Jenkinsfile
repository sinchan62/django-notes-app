pipeline {
    agent any

    stages {
        stage('clone code') {
            steps {
                echo 'clone code from github'
                git url: "https://github.com/sinchan62/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo 'build the packages'
                sh "docker build . -t todo-note-app"
            }
        }
        stage('push to dockerhub') {
            steps {
                echo 'push the code to docker hub'
                withCredentials([usernamePassword(credentialsId:"my-credential",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]) {
                    sh "docker tag todo-note-app ${env.dockerhubuser}/todo-note-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/todo-note-app:latest"
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy the code'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
