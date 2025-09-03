pipeline{
agent any 
stages{
stage(checkout) {
steps{
  git 'https://github.com/naren-30/java-project-maven-new.git'
}
}
    stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
stage(docker build image) {
steps{
sh 'docker build -t hotstar .'
}
}
stage(docker run) {
steps{
sh 'docker run -itd --name hotcont -p 8001:8080 hotstar'
}
}
}
}
