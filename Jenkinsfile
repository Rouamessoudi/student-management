pipeline {
    agent any

    /************************************************
     * Outils Jenkins (adapter les noms si besoin)
     * - Maven : doit exister dans "Manage Jenkins > Global Tool Configuration"
     * - JDK   : pareil
     ***********************************************/
    tools {
        maven 'Maven'      // nom configuré dans Jenkins (ex : "Maven", "Maven_3_9"...)
        jdk   'JDK17'      // nom du JDK (ex : "JDK17", "java-17"...)
    }

    environment {
        DOCKER_IMAGE = "rouamessoudi/student-management:1.0"
        KUBE_NAMESPACE = "default"   // tu peux mettre "devops" si tu utilises ce namespace
    }

    stages {

        stage('Git Checkout') {
            steps {
                // Récupérer le code depuis GitHub
                git branch: 'main',
                    url: 'https://github.com/Rouamessoudi/student-management.git'
            }
        }

        stage('Build avec Maven') {
            steps {
                sh '''
                    echo "=== Build Maven ==="
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh '''
                    echo "=== Docker build ==="
                    docker build -t $DOCKER_IMAGE .

                    echo "=== Docker push ==="
                    docker push $DOCKER_IMAGE
                '''
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                    echo "=== Déploiement des manifestes Kubernetes ==="
                    # MySQL + Spring Boot (les 2 fichiers YAML que tu as utilisés à la main)
                    kubectl apply -f mysql-deployment.yaml   -n $KUBE_NAMESPACE
                    kubectl apply -f spring-deployment.yaml  -n $KUBE_NAMESPACE
                '''
            }
        }

        stage('Deploy MySQL & Spring Boot on K8s') {
            steps {
                sh '''
                    echo "=== Vérification sur le cluster ==="
                    kubectl get pods -n $KUBE_NAMESPACE
                    kubectl get svc  -n $KUBE_NAMESPACE
                '''
            }
        }
    }
}
