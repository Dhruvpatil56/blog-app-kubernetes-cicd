pipeline {
    agent any

    tools {
        jdk 'jdk-17'
    }

    environment {
        IMAGE_NAME   = 'dhruvpatil56/blog-app'
        CLUSTER_NAME = 'eks-dev'
        REGION       = 'us-east-1'
        SCANNER_HOME = tool 'sonar-scanner'
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                      ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=blog-app \
                        -Dsonar.projectName=blog-app \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
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

        stage('Trivy Image Scan') {
            steps {
                sh '''
                  trivy image \
                    --severity HIGH,CRITICAL \
                    ${IMAGE_NAME}:latest
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
                  aws eks update-kubeconfig --region ${REGION} --name ${CLUSTER_NAME}

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
            echo '✅ DevSecOps CI/CD pipeline completed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}

