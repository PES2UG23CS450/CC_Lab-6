pipeline {
    agent any
    stages {
        stage('Build Backend Image') {
            steps {
                sh '''
                /usr/bin/docker rmi -f backend-app || true
                /usr/bin/docker build -t backend-app backend
                '''
            }
        }
        stage('Deploy Backend Containers') {
            steps {
                sh '''
                /usr/bin/docker network create app-network || true
                /usr/bin/docker rm -f backend1 backend2 || true
                /usr/bin/docker run -d --name backend1 --network app-network backend-app
                /usr/bin/docker run -d --name backend2 --network app-network backend-app
                '''
            }
        }
        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                /usr/bin/docker rm -f nginx-lb || true
                
                /usr/bin/docker run -d \
                  --name nginx-lb \
                  --network app-network \
                  -p 80:80 \
                  nginx
                
                /usr/bin/docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                /usr/bin/docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}
