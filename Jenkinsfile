pipeline {
    agent any

    stages {

        stage('Deploy Application') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Detect Deployment Failure') {
            steps {
                script {
                    def result = bat(
                        script: 'kubectl rollout status deployment/set14-web --timeout=30s',
                        returnStatus: true
                    )

                    if (result != 0) {
                        echo 'Deployment failure detected!'
                        echo 'Collecting troubleshooting information...'

                        bat 'kubectl get pods -l app=set14-web'
                        bat 'kubectl describe deployment set14-web'
                        bat 'kubectl describe pods -l app=set14-web'

                        echo 'Troubleshooting complete.'
                    }
                }
            }
        }

        stage('Fix Deployment') {
            steps {
                bat 'kubectl set image deployment/set14-web set14-nginx=nginx:1.25'
            }
        }

        stage('Verify Fix') {
            steps {
                bat 'kubectl rollout status deployment/set14-web --timeout=60s'
                bat 'kubectl get deployment set14-web'
                bat 'kubectl get pods -l app=set14-web'
            }
        }
    }
}