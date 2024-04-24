pipeline {
    agent any
    parameters {
        string(name: 'DOCKER_IMAGE_TAG', defaultValue: 'jenkins', description: 'Docker image tag')
    }
    environment {
        DOCKER_IMAGE_NAME = 'csuvikg/dockertest'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/csuvikg/jenkins-starter'
            }
        }
        stage('Test step') {
            steps {
                parallel(
                    'A stage': {
                        sh 'sleep $((RANDOM % 20))'
                    },
                    'Another stage': {
                        sh 'sleep $((RANDOM % 20))'
                    }
                )
            }
        }
        stage('Configure') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t ${env.DOCKER_IMAGE_NAME}:${params.DOCKER_IMAGE_TAG} ."
            }
        }
        stage('Deploy') {
            steps {
                sh "docker push ${env.DOCKER_IMAGE_NAME}:${params.DOCKER_IMAGE_TAG}"
            }
        }
    }
}
