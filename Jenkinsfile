pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "carlossuni/banking-app:latest"
    }

    stages {
    
    	stage('Run Tests') {
    	    steps {
    	        sh 'docker run --rm -v "$PWD":/app -w /app python:3.11-slim sh -c "pip install -r requirements.txt && pytest"'
    	    }
    	}
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh 'k3s kubectl apply -f deployment.yaml --validate=false'
                sh 'k3s kubectl apply -f service.yaml --validate=false'
                sh 'k3s kubectl rollout restart deployment banking-app'
            }
        }
    }
}
