pipeline {

agent any

environment {

IMAGE_NAME="madhu58/project"

CONTAINER="project"

}

stages {

stage('Checkout'){

steps{

git 'https://github.com/madhusudhan58/Project.git'

}

}

stage('Build Docker'){

steps{

sh 'docker build -t $IMAGE_NAME:latest .'

}

}

stage('Docker Login'){

steps{

withCredentials([usernamePassword(credentialsId:'dockerhub',

usernameVariable:'USER',

passwordVariable:'PASS')]){

sh '''

echo $PASS | docker login -u $USER --password-stdin

'''

}

}

}

stage('Push Image'){

steps{

sh 'docker push $IMAGE_NAME:latest'

}

}

stage('Deploy'){

steps{

sh '''

docker stop website || true

docker rm website || true

docker run -d --name website -p 80:80 $IMAGE_NAME:latest

'''

}

}

}

}