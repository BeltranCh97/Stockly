pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "beltranch97/beltran-app:latest"
        // ID de la credencial Kubeconfig configurada en Jenkins
        KUBE_CRED_ID = 'local-kubeconfig' 
    }

    stages {
        stage('Descargar Configuración') {
            steps {
                // Descarga los archivos .yaml del repositorio para que Jenkins los lea
                checkout scm [cite: 39]
            }
        }

        stage('Despliegue en Kubernetes') {
            steps {
                script {
                    // Se realiza el despliegue dentro de kubernetes
                    withKubeConfig([credentialsId: "${KUBE_CRED_ID}"]) {
                        sh "kubectl apply -f k8s/deployment.yaml"
                        sh "kubectl rollout restart deployment mi-app-devops"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "¡Despliegue completado con éxito! La aplicación está operativa." [cite: 16]
        }
    }
}