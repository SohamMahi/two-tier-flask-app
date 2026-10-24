pipeline{
    agent {label "dev"};
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/SohamMahi/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh " docker build -t my-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Code Test done"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcreds",
                    passwordVariable: "dockerPass",
                    usernameVariable: "dockerUser"
                    )]){
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                    sh "docker image tag my-flask-app ${env.dockerUser}/my-flask-app"
                    sh "docker push ${env.dockerUser}/my-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
