pipeline {
    agent any
    stages {
        stage('Build Account') {
            steps {
                build job: 'api.account', wait: true
            }
        }
        
        stage('Build') { 
            steps {
                sh 'mvn clean package'
            }
        }      

        stage('Build Image') {
            steps {
                script {
                    account = docker.build("pasilva2023/account:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credential') {
                        account.push("${env.BUILD_ID}")
                        account.push("latest")
                    }
                }
            }
        }
    }
}