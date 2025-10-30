pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'naren3005'     // your Docker Hub username
        IMAGE_NAME = 'hotstar'            // your Docker Hub repository
        IMAGE_TAG = 'v1'
        K8S_DEPLOYMENT = 'hotstar-deployment'
        K8S_SERVICE = 'hotstar-service'
        K8S_NAMESPACE = 'default'          // change if you use another namespace
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/naren-30/java-project-maven-new.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker',  // Jenkins credential ID
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG_FILE

                    echo "Applying Kubernetes Deployment..."
                    kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: $K8S_DEPLOYMENT
  namespace: $K8S_NAMESPACE
spec:
  replicas: 3
  selector:
    matchLabels:
      app: $IMAGE_NAME
  template:
    metadata:
      labels:
        app: $IMAGE_NAME
    spec:
      containers:
      - name: $IMAGE_NAME
        image: $DOCKER_HUB_USER/$IMAGE_NAME:$IMAGE_TAG
        ports:
        - containerPort: 8008
EOF

                    echo "Applying Kubernetes Service..."
                    kubectl apply -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: $K8S_SERVICE
  namespace: $K8S_NAMESPACE
spec:
  selector:
    app: $IMAGE_NAME
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8008
      nodePort: 30008
  type: NodePort
EOF

                    kubectl rollout status deployment/$K8S_DEPLOYMENT -n $K8S_NAMESPACE
                    kubectl get all -n $K8S_NAMESPACE
                    '''
                }
            }
        }
    }
}
