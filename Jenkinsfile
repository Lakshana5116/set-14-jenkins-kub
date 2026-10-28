pipeline {
    agent any

    stages {

        stage('Deploy Application') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Check Deployment') {
            steps {
                bat 'kubectl get deployment set14-web'
                bat 'kubectl get pods -l app=set14-web'
            }
        }

        stage('Troubleshoot Deployment') {
            steps {
                bat 'kubectl describe deployment set14-web'
            }
        }

        stage('Check Rollout') {
            steps {
                bat 'kubectl rollout status deployment/set14-web --timeout=30s'
            }
        }
    }
}