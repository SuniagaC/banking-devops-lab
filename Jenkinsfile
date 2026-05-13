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
                sh 'docker save banking-app:latest | sudo k3s ctr images import -'
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f deployment.yaml'
                sh 'sudo kubectl apply -f service.yaml'
            }
        }

    }
}
