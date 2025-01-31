pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/playpalash/racingapp'
        BRANCH = 'master' // Change this if your default branch is different
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("playpalash/car-racing:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'e1ed803c-8073-4c7e-85a2-d6c96bb4f6e9') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'k8s/deployment.yaml',
                        kubeconfigId: '5180a9fb-62e4-4009-a2c4-2f7672b845d0'
                    )
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

