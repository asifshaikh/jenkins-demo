pipeline{
    agent any

    environment{
        IMAGE_NAME = "jenkins-demo"
        DOCKERHUB_REPO = "sekc69/${IMAGE_NAME}"
        CONTAINER_NAME = "flask_app"
    }
    stages{
        stage('Checkout Code'){
            steps{
                git branch: 'main', url: 'https://github.com/asifshaikh/jenkins-demo.git'
                echo 'Code checked out successfully'
            }
        }
        stage('Run Tests inside Docker Container'){
            steps{
                sh '''
                docker run --rm \
                -v "$PWD:/app" \
                -w /app \
                python:3.9-slim \
                sh -c "pip install -r requirements.txt && pytest"
                '''
            }
        }
        stage('Build docker image'){
            steps{
                sh '''
                docker build -t ${IMAGE_NAME}:latest .
                docker tag ${IMAGE_NAME}:latest ${DOCKERHUB_REPO}:latest
                '''
                echo 'Docker image built successfully'
            }
        }
        stage('Finish'){
            steps{
                echo 'Finished'
            }
        }
    }
}