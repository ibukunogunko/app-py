pipeline {
    environment {
        registry = 'ibukunola/docker-jenkins'
        registryCredentials = 'DOCKER-HUB-ACCESS-TOKEN'
    }
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Cloning our github repository') {
            steps {
                git credentialsId: 'GIT-ACCESS-TOKEN', url: 'https://github.com/ibukunogunko/app-py.git', branch: 'main'
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t app-py-image .'
            }
        }
        stage('Tag docker image') {
            steps {
                sh 'docker tag app-py-image:latest ibukunola/docker-jenkins:$BUILD_NUMBER'
                sh 'docker tag app-py-image:latest ibukunola/docker-jenkins:latest'
            }
        }
        stage('Push image to Docker Repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER-HUB-ACCESS-TOKEN', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                    sh 'docker push $registry:$BUILD_NUMBER'
                    sh 'docker push $registry:latest'
                }
            } 
        }
    }
}    
