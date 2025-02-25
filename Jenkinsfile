pipeline{
    agent {label 'dev'}
    stages{
        stage("code"){
            steps {
                git url: "https://github.com/Swayamnakshane/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("build"){
            steps {
                sh "docker build -t flaskapp ."
            }
        }
        stage("push to docker hub"){
            steps {
                  withCredentials([usernamePassword(
                    credentialsId:"dockerhubcred",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag flaskapp ${env.dockerHubUser}/flaskapp"
                sh "docker push ${env.dockerHubUser}/flaskapp:latest"
                }
            }
        }
        stage("deploy"){
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
post {
    success {
        script {
            emailext(
                subject: "Build Success",
                body: "Good news! The build was successful.",
                to: 'swayamvictus1803@gmail.com'
            )
        }
    }
    failure {
        script {
            emailext(
                subject: "Build Failed",
                body: "Bad news! The build failed.",
                to: 'swayamvictus1803@gmail.com'
            )
        }
    }
}
}
