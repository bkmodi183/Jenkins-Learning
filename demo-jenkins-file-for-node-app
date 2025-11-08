pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bkmodi183/todo-app-jenkins-docker-node-2.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker build . -t bkmodi183/todo-node-app-jenkins-test:latest"
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'dockerHub',
                usernameVariable: 'dockerHubUser',
                passwordVariable: 'dockerHubPassword')])
                {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh "docker push bkmodi183/todo-node-app-jenkins-test:latest"
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the new build'
            }
        }

        stage('Deploy') {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
