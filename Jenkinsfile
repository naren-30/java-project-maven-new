pipeline{
agent any 
stages{
stage(checkout){
steps{
  git 'https://github.com/naren-30/java-project-maven-new.git'
}
}
    stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
stage(docker build image){
steps{
docker build -t hotstar .
}
}
stage(docker run){
steps{
docker run -itd --name hotstar -p 8001:8080 hotstar
}
}
}
}
