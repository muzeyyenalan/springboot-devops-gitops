pipeline {
    agent any

    environment {
        APP_NAME = 'devops-demo'
        IMAGE_TAG = '3.0'
        MAVEN_IMAGE = 'maven:3.9-eclipse-temurin-21'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub.'
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker run --rm \
                      -v "$WORKSPACE:/app" \
                      -w /app \
                      maven:3.9-eclipse-temurin-21 \
                      mvn test
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    docker run --rm \
                      -v "$WORKSPACE:/app" \
                      -w /app \
                      maven:3.9-eclipse-temurin-21 \
                      mvn package -DskipTests
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-demo:3.0 .'
                sh 'docker image inspect devops-demo:3.0'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Maven tests, package and Docker build completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}