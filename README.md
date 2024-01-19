Jenkins Declarative Pipeline code:
pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/ambala00/Kubernetes_miniproject.git'
            }
        }
        stage('Build the Docker Image') {
            steps {
                sh 'docker build -t ambalavaneshwaran/Kubeimage:latest /var/lib/jenkins/workspace/Kubernetes_project-1'
                sh 'docker tag ambalavaneshwaran/kubeimage:latest ambalavaneshwaran/kubeimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'docker image push ambalavaneshwaran/kubeproject:latest'
                sh 'docker image push ambalavaneshwaran/kubeproject:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/Kubernetes_project-1/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
