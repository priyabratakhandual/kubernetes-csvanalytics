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
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Deployment completed successfully.</p>
                         <p>Job: ${env.JOB_NAME}</p>
                         <p>Build Number: ${env.BUILD_NUMBER}</p>
                         <p>URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: "Kanishk.kumar@Globtier.in, Sachin.Srivastav@globtierinfotech.com"
            )
        }

        failure {
            echo '❌ Deployment failed.'
            emailext(
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Deployment failed.</p>
                         <p>Job: ${env.JOB_NAME}</p>
                         <p>Build Number: ${env.BUILD_NUMBER}</p>
                         <p>URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: "Kanishk.kumar@Globtier.in, Sachin.Srivastav@globtierinfotech.com"
            )
        }
    }
}
