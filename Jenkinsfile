pipeline {
    agent any

    environment {
        KUBECTL = 'C:\\Users\\admin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe'
        KUBECONFIG = 'C:\\Users\\admin\\.kube\\config'
    }

    stages {
        stage('Check Kubernetes') {
            steps {
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" get nodes'
            }
        }

        stage('Update Web Content') {
            steps {
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" create configmap set11-web-content --from-file=index.html=index.html --dry-run=client -o yaml | "%KUBECTL%" --kubeconfig="%KUBECONFIG%" apply -f -'
            }
        }

        stage('Deploy Application') {
            steps {
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" apply -f k8s/deployment.yaml'
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" rollout restart deployment/set11-web'
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" rollout status deployment/set11-web --timeout=180s'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" get deployment set11-web'
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" get pods -l app=set11-web'
                bat '"%KUBECTL%" --kubeconfig="%KUBECONFIG%" get service set11-web-service'
            }
        }
    }
}
