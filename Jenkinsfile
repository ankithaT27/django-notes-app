pipeline{
    agent any
    
    stages{
        stage("code")
        {
            steps
            {
                echo "code from github"
                git url:"https://github.com/ankithaT27/django-notes-app.git" , branch:"main"
                
            }
        }
        
        stage("build")
        {
            steps
            {
                echo "build using dockerfile"
                sh "pwd"
                sh "docker build . -t my-note-app"
            }
        }
        
        stage("push to docker")
        {
            steps
            {
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                sh "docker run -d -p 8000:8000 ankitha2798/my-note-app"
                }
            }
        }
        
        stage("deploy")
        {
            steps
            {
                echo "deploy to dockerhub"
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
