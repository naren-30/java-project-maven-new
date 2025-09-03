pipeline {
    agent any

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
    }
}
