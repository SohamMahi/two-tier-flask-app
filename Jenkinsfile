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
                script{
                    trivy()
                }
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
                script{
                    docker-push("dockerhubcreds", "my-flask-app")
                }
            }
        }
        stage("Deploy"){
            steps{
                script{
                    docker-compose()
                }
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
