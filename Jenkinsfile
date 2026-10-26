pipeline {
    agent any

    environment {
        APP_NAME = 'devops-demo'
        IMAGE_TAG = '3.0'
        REPO_URL = 'https://github.com/muzeyyenalan/springboot-devops-gitops.git'
        MAVEN_IMAGE = 'maven:3.9-eclipse-temurin-21'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Test and Build') {
            steps {
                sh '''
                    docker run --rm \
                      -e REPO_URL="$REPO_URL" \
                      -e BRANCH_NAME=master \
                      "$MAVEN_IMAGE" \
                      sh -c '
                        apt-get update -qq &&
                        apt-get install -y -qq git &&
                        git clone --branch "$BRANCH_NAME" "$REPO_URL" /tmp/app &&
                        cd /tmp/app &&
                        mvn test &&
                        mvn package -DskipTests
                      '
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Docker build will be enabled after the workspace sharing is corrected.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Maven test and package completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}