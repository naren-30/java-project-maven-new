pipeline{
agent any 
stages{
stage(checkout){
steps{
  git 'https://github.com/naren-30/java-project-maven-new.git'
}
}
stage(clean package){
steps{
sh 'mvn clean package'
}
}
stage(docker build image){
steps{
docker build -t hotstare .
}
}
stage(docker run){
steps{
docker run -itd --name hotstar -p 8001:8080 hotstar
}
}
}
}
