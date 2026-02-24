pipeline {
    agent {label 'Agent1'}
    
    environment {
        FRONTEND = "ukarthikeyan/ddtask_frontend"
        BACKEND = "ukarthikeyan/ddtask_backend"
        TAG = "latest"
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    docker.build("${FRONTEND}:${TAG}", "-f frontend/Dockerfile frontend")
                    docker.build("${BACKEND}:${TAG}", "-f backend/Dockerfile backend")
                }
            }
        }
        
        stage('Push Image') {
            steps{
                script {
                    docker.withRegistry('https://index.docker.io/v1/','MyDocker_creds') {
                         docker.image("${FRONTEND}:${TAG}").push()
                         docker.image("${BACKEND}:${TAG}").push()
                    }
                }
            }
        }
    
        
        stage('Deploy Containers') {
            steps {
                sh '''
                   docker-compose down
                   docker-compose pull && docker-compose up -d
                   docker image prune -f
                '''
            }
        }
    }

    post {
        success {
            echo "Successfully deploy the DD_TASK Containerized application"
        }
        failure {
            echo "Deployment failed"
        }
    }
}
