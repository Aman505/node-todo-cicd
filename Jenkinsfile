pipeline{
    agent {label 'dev-worker'}
    stages{
        stage('Clone'){
            steps{
            git url: 'https://github.com/Aman505/node-todo-cicd.git', branch: 'master'
            
            }
        }
        stage('Build'){
            steps{
                sh 'docker build -t note-app .'
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcreds",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag note-app:latest ${env.dockerHubUser}/note-app:latest"
                sh "docker push ${env.dockerHubUser}/note-app:latest"
                }
            }
        }
        stage('deploy'){
            steps{
                sh 'docker compose down && docker compose up -d --build'
            }
        }
    }
}
