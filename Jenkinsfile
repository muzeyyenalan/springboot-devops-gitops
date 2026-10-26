pipeline {
    agent any

    environment {
        APP_NAME = 'devops-demo'
        IMAGE_TAG = '2.0'
        K8S_NAMESPACE = 'jenkins-demo'
        K8S_SERVER = 'https://kubernetes.docker.internal:6443'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is provided by the Jenkins workspace.'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-demo:2.0 .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Kubernetes deployment will be added after validating credentials and image availability.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Build and test stages completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}