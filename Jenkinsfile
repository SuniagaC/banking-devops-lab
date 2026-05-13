pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t banking-app:latest .'
            }
        }

        stage('Import Image to k3s') {
            steps {
                sh 'docker save banking-app:latest | k3s ctr images import -'
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
