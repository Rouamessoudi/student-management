pipeline {
    agent any

    tools {
        maven 'M3'
        jdk   'JDK17'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
        IMAGE_NAME = "rouamessaoudi/student-management:1.0"
    }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/Rouamessaoudi/student-management.git', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME} .
                    echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                    docker push ${IMAGE_NAME}
                """
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh """
                    kubectl apply -f k8s/mysql-deployment.yaml
                    kubectl apply -f k8s/spring-deployment.yaml
                """
            }
        }

        stage('Deploy MySQL & Spring Boot on K8s') {
            steps {
                sh """
                    kubectl get pods
                    kubectl get svc
                """
            }
        }
    }
}
