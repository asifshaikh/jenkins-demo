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
        stage('Test'){
            steps{
                echo 'Testing..'
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploying....'
            }
        }
        stage('Finish'){
            steps{
                echo 'Finished'
            }
        }
    }
}