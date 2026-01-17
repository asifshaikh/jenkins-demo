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
        stage('Push to Docker Hub'){
            steps{
                withCredentials([usernamePassword(credentialsId:'dockerHubCreds', usernameVariable:'DOCKERHUB_USERNAME', passwordVariable:'DOCKERHUB_PASSWORD')]){
                    sh '''
                    echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
                    docker push ${DOCKERHUB_REPO}:latest
                    '''
                }
            }
        }
        stage('Deploy to local Docker'){
            steps{
                sh '''
                docker rm -f ${CONTAINER_NAME} || true
                docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${DOCKERHUB_REPO}:latest
                '''
                echo 'Application deployed successfully'
            }
        }
        
    }
    post {
        success {
            echo "🚀 Full CI/CD pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}