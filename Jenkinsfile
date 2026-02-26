pipeline {
    agent any

    environment {
        DOCKER = "/usr/bin/docker"
    }

    stages {

        stage('Build Backend Image') {
            steps {
                sh '''
                $DOCKER --version || true
                $DOCKER rmi -f backend-app || true
                $DOCKER build -t backend-app backend
                '''
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh '''
                $DOCKER network create app-network || true
                $DOCKER rm -f backend1 backend2 || true
                $DOCKER run -d --name backend1 --network app-network backend-app
                $DOCKER run -d --name backend2 --network app-network backend-app
                '''
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                $DOCKER rm -f nginx-lb || true

                $DOCKER run -d \
                  --name nginx-lb \
                  --network app-network \
                  -p 80:80 \
                  nginx

                $DOCKER cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                $DOCKER exec nginx-lb nginx -s reload
                '''
            }
        }

    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
