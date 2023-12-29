pipeline {
    agent any

    stages {
        
        stage('code') {
            
            steps {
                
                echo 'cloning the code'
                git url : "https://github.com/Randeepk1/django-notes-app.git",branch: "main"
            }
        }
        
        stage('build') {
            steps {
                echo 'building the image '
                sh "docker build -t my-node-app . " 
            }
        }
        stage("push to docker hub") {
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"Docker-Hub",passwordVariable:"dockerhubpassword",usernameVariable:"DockerHubUser")]){
                sh "docker tag my-node-app ${env.DockerHubUser}/my-node-app:latest"
                sh "docker login -u ${env.DockerHubUser} -p ${env.dockerhubpassword}"
                sh "docker push ${env.DockerHubUser}/my-node-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the docker container'
                sh "docker compose down && docker-compose up -d"
                //sh "docker run -d -p 8000:8000 dockeruserran/my-node-app:latest"
                } 
            }
    }
}
