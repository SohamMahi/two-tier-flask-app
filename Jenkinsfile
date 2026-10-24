@Library("Shared") _
pipeline{
    agent {label "dev"};
        
    stages{
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/SohamMahi/two-tier-flask-app.git", "master")
                }
            }
        }
        stage("Trivy File scan"){
            steps{
                sh "trivy fs . -o results.json"
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
    
post{
    success{
            emailext body: 'Good News!! Build is Successfull!!!!',
                subject: 'Build Success!!!',
                to: 'sohammahi1@gmail.com'
    }
    failure{
            emailext body: 'Bad News!! Build is FAILED!!!!',
                subject: 'Build Failed!!!',
                to: 'sohammahi1@gmail.com'
    }
}
}
