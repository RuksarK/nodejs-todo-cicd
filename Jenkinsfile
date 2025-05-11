pipeline {
    agent any
    
    stages{
        stage("code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/RuksarK/nodejs-todo-cicd.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t todo-app ."
            }
        }
        stage("Push"){
            steps{
                echo "Push the code on docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag todo-app ${env.dockerHubUser}/todo-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/todo-app:latest"
                }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
