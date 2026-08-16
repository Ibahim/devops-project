 pipeline {

    agent any

    environment {
        APP_NAME = "devops-app"
        CONTAINER_NAME = "devops-container-v2"
        HOST_PORT = "8082"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ibahim/devops-project.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true

                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:80 \
                    ${APP_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    echo "Checking application health..."

                    curl -f http://localhost:${HOST_PORT}

                    echo "Health Check PASSED!"
                '''
            }
        }
    }
}
