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
                    docker run --name devops-demo-maven-build \
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

                    docker cp devops-demo-maven-build:/tmp/app/target/demo-0.0.1-SNAPSHOT.jar "$WORKSPACE/target/demo-0.0.1-SNAPSHOT.jar"

                    docker rm devops-demo-maven-build
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
            sh 'docker rm -f devops-demo-maven-build 2>/dev/null || true'
            echo 'Pipeline execution completed.'
        }

        success {
            echo 'Tests, package and Docker image build completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}