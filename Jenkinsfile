pipeline {
    agent { label 'Dev-Agent' }
    
    stages{
        stage('code'){
            steps {
                git url: 'https://github.com/abhayzadoo/node-todo-demo', branch: 'main'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t devabhay/node-todo-demo-cicd:latest'
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'DockerHub',passwordVariable:'DockerHubPassword', usernameVariable:'DockerHubUsername')]) {
                    sh "docker login -u ${env.DockerHubUsername} -p ${env.DockerHubPassword}"
                    sh "docker push devabhay/node-todo-demo-cicd:latest"
                }    
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
