pipeline {
    agent any

    tools {
        jdk 'jdk-17'
    }

    environment {
        IMAGE_NAME = 'dhruvpatil56/blog-app'
        CLUSTER    = 'eks-dev'
        REGION     = 'us-east-1'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Dhruvpatil56/blog-app-kubernetes-cicd.git'
            }
        }

        stage('Build Application') {
            steps {
                sh '''
                  chmod +x mvnw
                  ./mvnw clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                  docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                      docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                  aws eks update-kubeconfig --region ${REGION} --name ${CLUSTER}

                  kubectl apply -f k8s/
                  kubectl set image deployment/blog-app \
                    blog-app=${IMAGE_NAME}:${BUILD_NUMBER}

                  kubectl rollout status deployment/blog-app
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Application successfully deployed to EKS'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}

