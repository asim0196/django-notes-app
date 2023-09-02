pipeline {
    agent  {
        label 'node_1'
    }
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/asim0196/django-notes-app.git", branch: "main"
            }
       }
       stage("Build"){
            steps {
                echo "Building the code"
                sh '''
                sudo apt update
                sudo apt install docker.io -y
                sudo apt install docker-compose -y
                docker build -t my-note-app .
                
                '''
            }    
       }
       stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"Pass",usernameVariable:"User")]){
                sh "docker tag my-note-app ${env.User}/my-note-app:latest"
                sh "docker login -u ${env.User} -p ${env.Pass}"
                sh "docker push ${env.User}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }

    }
}
