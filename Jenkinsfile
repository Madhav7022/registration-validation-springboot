
pipeline {
    agent any

    environment {
        IMAGE_NAME = "madhav7022/spring_project2003"
        TAG = "latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:$TAG'
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }

        stage('Remove Local Image') {
            steps {
                sh 'docker rmi $IMAGE_NAME:$TAG'
            }
        }

        stage('Trigger Ansible Playbook') {
            steps {
                sh 'ansible-playbook deploy.yml'
            }
        }
    }
}
