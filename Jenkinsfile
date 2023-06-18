pipeline {
    agent { label "dev-agent" }
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/swapnilwagh2204/node-todo-cicd-master.git", branch: "main"
            }
        }
        stage("Build and Test"){
            steps{
                echo "buidling and testing the app"
                sh "docker build . -t ${env.dockerHubUser}/node-app-test-new"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
