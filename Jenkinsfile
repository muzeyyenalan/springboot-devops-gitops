
pipeline {
    agent any

    environment {
        APP_NAME = 'devops-demo'
        IMAGE_TAG = '3.0'
        REPO_URL = 'https://github.com/muzeyyenalan/springboot-devops-gitops.git'
        MAVEN_IMAGE = 'maven:3.9-eclipse-temurin-21'
        KUBE_CONFIG = '/var/jenkins_home/.kube/config'
        KUBE_NAMESPACE = 'jenkins-demo'
        DEPLOYMENT_NAME = 'devops-demo'
        CONTAINER_NAME = 'devops-demo'
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
                    docker rm -f devops-demo-maven-build 2>/dev/null || true

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

                    mkdir -p "$WORKSPACE/target"
                    docker cp devops-demo-maven-build:/tmp/app/target/demo-0.0.1-SNAPSHOT.jar "$WORKSPACE/target/demo-0.0.1-SNAPSHOT.jar"
                    docker rm -f devops-demo-maven-build
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t "$APP_NAME:$IMAGE_TAG" .
                    docker image inspect "$APP_NAME:$IMAGE_TAG"
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl --kubeconfig="$KUBE_CONFIG" \
                      set image deployment/"$DEPLOYMENT_NAME" \
                      "$CONTAINER_NAME"="$APP_NAME:$IMAGE_TAG" \
                      -n "$KUBE_NAMESPACE"

                    kubectl --kubeconfig="$KUBE_CONFIG" \
                      rollout status deployment/"$DEPLOYMENT_NAME" \
                      -n "$KUBE_NAMESPACE" --timeout=120s
                '''
            }
        }
    }

    post {
        always {
            sh 'docker rm -f devops-demo-maven-build 2>/dev/null || true'
            echo 'Pipeline execution completed.'
        }

        success {
            echo 'Tests, Docker build and Kubernetes deployment completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
