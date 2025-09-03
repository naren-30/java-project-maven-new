
pipeline {
    agent any

    environment {
       DOCKER_HUB_USER = 'naren3005'
        IMAGE_NAME ='hotstar'
        IMAGE_TAG = 'v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/naren-30/java-project-maven-new.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t hotstar .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f hotcont || true
                docker run -d --name hotcont -p 8001:8080 hotstar
                '''
            }
        }
        stage (docker swarm) {
            steps{
                '''
                docker sevice create --name hotserv -p 8008:8080 --replicas 9 hotstar
                '''
            }   
        }
        stage('push to dockerhub') {
          steps{
              withCredentials([usernamePassword(
                  credentialsId: 'docker', //jenkins credentials ID
                  usernameVareable: 'DOCKER_USER',
                  passwordVariables: 'DOCKER_PASS'
              )])
          }
        }
    }
}
