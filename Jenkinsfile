pipeline {
    agent any

    environment {
        IMAGE_NAME = 'priyabratakhandual/csvanalytics:latest'
        DEPLOYMENT_FILE = 'deployment.yaml'
        SERVICE_FILE = 'service.yaml'
    }

    stages {
        stage('Checkout') {
            steps {
                // If your deployment files are in Git
                git 'https://github.com/priyabratakhandual/kubernetes-csvanalytics.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    echo "Deploying $IMAGE_NAME to Kubernetes..."

                    kubectl apply -f $DEPLOYMENT_FILE
                    kubectl apply -f $SERVICE_FILE

                    echo "Waiting for pod to be ready..."
                    kubectl rollout status deployment/csvanalytics-deployment
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully.'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
