pipeline {
    agent any

    /************************************************
     * Outils Jenkins (adapter les noms si besoin)
     * - Maven : doit exister dans "Manage Jenkins > Global Tool Configuration"
     * - JDK   : pareil
     ***********************************************/
   tools {
    maven 'M2_HOME'
    jdk   'JAVA_HOME'
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
    withEnv(["PATH+MAVEN=${tool 'Maven_3_9'}/bin", "JAVA_HOME=${tool 'JDK17'}"]) {
      script {
        sh """
          echo "=== Docker build ==="
          docker build -t rouamessaoudi/student-management:1.0 .
          echo "=== Docker push ==="
          docker push rouamessaoudi/student-management:1.0
        """
      }
    }
  }
}    stage('Docker Build & Push') {
        steps {
            sh 'echo "=== Docker build ==="'
            sh 'docker build -t rouamessoudi/student-management:1.0 .'

            sh 'echo "=== Docker push ==="'
            sh 'docker push rouamessoudi/student-management:1.0'
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
