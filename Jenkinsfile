pipeline {
  agent any

  environment {
    IMAGE_NAME = "flask-k8s"
    IMAGE_TAG = "1.0"
    NAMESPACE = "k8s-demo"
  }

  stages {
    stage("Checkout") {
      steps {
        sh "ls -la"
      }
    }

    stage("Build Docker Image") {
      steps {
        sh "kubectl apply -f k8s/namespace.yaml"
        sh "kubectl apply -f k8s/deployment.yaml"
        sh "kubectl apply -f k8s/service.yaml"
        sh "kubectl apply -f k8s/ingress.yaml || true"
        sh "kubectl rollout restart deployment/flask-app -n ${NAMESPACE}"
        sh "kubectl rollout status deployment/flask-app -n ${NAMESPACE}"
      }
    }

    stage("Verify") {
      steps {
        sh "kubectl get pods -n ${NAMESPACE}"
        sh "kubectl get svc -n ${NAMESPACE}"
      }
    }
  }
}
