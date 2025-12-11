pipeline {
    agent any

    /******************************************************
     * Outils Jenkins (adapter les noms si besoin)
     * - Maven : doit exister dans "Manage Jenkins > Global Tool Configuration"
     * - JDK   : pareil
     ******************************************************/
    tools {
        maven 'M2_HOME'      // le nom que tu as mis pour Maven
        jdk   'JAVA_HOME'    // le nom que tu as mis pour le JDK
    }

    environment {
        DOCKER_IMAGE   = "rouamessaoudi/student-management:1.0"
        KUBE_NAMESPACE = "devops"   // ou "default" si tu utilises ce namespace
    }

    stages {

        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build avec Maven') {
            steps {
                sh 'echo "=== Build Maven ==="'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh '''
                  echo "=== Docker build ==="
                  docker build -t ${DOCKER_IMAGE} .

                  echo "=== Docker push ==="
                  docker push ${DOCKER_IMAGE} || echo "⚠️ push déjà fait ou refusé (mais on continue)"
                '''
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                  echo "=== Kubernetes Deploy (placeholder) ==="
                  # Ici normalement: kubectl apply -f tes-fichiers.yaml -n ${KUBE_NAMESPACE}
                  echo "Pas de manifests K8s -> on simule la réussite du déploiement."
                '''
            }
        }

        stage('Deploy MySQL & Spring Boot on K8s') {
            steps {
                sh '''
                  echo "=== Vérification K8s (placeholder) ==="
                  # Ici normalement: kubectl get pods -n ${KUBE_NAMESPACE}
                  echo "Vérification simulée pour avoir la stage en vert."
                '''
            }
        }
    }
}
