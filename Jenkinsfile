pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = 'aws-eks-pipeline'
        EKS_CLUSTER = 'aws-eks-pipeline'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    env.AWS_ACCOUNT_ID = sh(script: 'aws sts get-caller-identity --query Account --output text', returnStdout: true).trim()
                    env.ECR_URI = "${env.AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
                    sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to ECR') {
            steps {
                echo 'Pushing Docker image to AWS ECR...'
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${env.ECR_URI}"
                    sh "docker tag ${ECR_REPO}:${IMAGE_TAG} ${env.ECR_URI}:${IMAGE_TAG}"
                    sh "docker tag ${ECR_REPO}:${IMAGE_TAG} ${env.ECR_URI}:latest"
                    sh "docker push ${env.ECR_URI}:${IMAGE_TAG}"
                    sh "docker push ${env.ECR_URI}:latest"
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                echo 'Deploying to AWS EKS...'
                script {
                    sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${EKS_CLUSTER}"
                    sh "sed -i 's|AWS_ACCOUNT_ID|${env.AWS_ACCOUNT_ID}|g' k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                    sh "kubectl rollout status deployment/aws-eks-pipeline"
                }
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully! App is deployed on EKS.'
        }
        failure {
            echo 'Pipeline failed! Check the logs above.'
        }
    }
}