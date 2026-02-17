pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh '''
                docker rmi -f backend-app || true
                docker build -t backend-app backend
                '''
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker network create lab6-net || true
                docker rm -f backend1 backend2 || true
                docker run -d --network lab6-net --name backend1 backend-app
                docker run -d --network lab6-net --name backend2 backend-app
                '''
            }
        }

        stage('Build NGINX LB') {
            steps {
                sh '''
                docker rmi -f nginx-lb-image || true
                docker build -t nginx-lb-image nginx
                '''
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                docker run -d --network lab6-net -p 80:80 --name nginx-lb nginx-lb-image
                '''
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed. Check console logs for errors."
        }
        success {
            echo "Pipeline completed successfully!"
        }
    }
}

