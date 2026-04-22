docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts




  docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword 


  pipeline {
    agent any

    environment {
        DOCKER_USERNAME = credentials('DOCKER_USERNAME')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        GROQ_API_KEY = credentials('GROQ_API_KEY')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Code checked out successfully'
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t $DOCKER_USERNAME/infrachat-backend:latest ./backend'
                echo 'Backend image built successfully'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t $DOCKER_USERNAME/infrachat-frontend:latest ./frontend'
                echo 'Frontend image built successfully'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm $DOCKER_USERNAME/infrachat-backend:latest python3 -c "import fastapi; print(\"Backend OK\")"'
                echo 'Tests passed successfully'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                sh 'docker push $DOCKER_USERNAME/infrachat-backend:latest'
                sh 'docker push $DOCKER_USERNAME/infrachat-frontend:latest'
                echo 'Images pushed to Docker Hub'
            }
        }

        stage('Deploy') {
            steps {
                sh 'curl -X POST $RENDER_DEPLOY_HOOK'
                echo 'Deployed to Render successfully'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
