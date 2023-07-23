pipeline {
    agent any
    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code'
                git url:"https://github.com/nitinchopade/django-notes-app.git", branch:"main"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the code'
                sh "docker build -t my-note-app ."
            }
        }
        stage('Push Image to Docker-Hub') {
            steps {
                echo 'Pushing image to docker-hub'
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                sh "docker tag my-note-app ${env.dockerHubuser}/my-note-app:latest"
                sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                sh "docker push ${env.dockerHubuser}/my-note-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the container'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
