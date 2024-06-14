pipeline {
    agent any
    stages{
        stage('Checkout git') {
            steps {
              git 'https://github.com/ShMrunal/medicure-app.git'
            }
        }
        
        stage ('Build & JUnit Test') {
            steps {
                sh 'mvn clean package' 
            }
        }
        
        stage("Build Docker Image") {
            steps {
                echo "Building the image"
                catchError(buildResult: 'UNSTABLE') {
                    sh "docker build -t ${env.dockerHubUser}/medicure-app:latest ."
                }
            }
        }
        
        stage("Push To Docker Hub") {
            steps {
                echo "pushing to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                   sh "docker tag medicure-app ${env.dockerHubUser}/medicure-app:latest"
                   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                   sh "docker push ${env.dockerHubUser}/medicure-app:latest"
                }
            }
        }
    }
}