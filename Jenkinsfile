pipeline {
    agent any

    stages {
        stage("Code clone") {
            steps {
                sh "whoami"
                git url: 'https://github.com/jaya855/MemoBox.git', branch: 'main'
            }
        }

        stage("Code Build") {
            steps {
                sh "docker build -t my-note-app ."
            }
        }

        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
