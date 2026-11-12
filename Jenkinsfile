pipeline {
    agent any

    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('gcp-service-account') 
        GKE_CLUSTER_NAME = 'zomoto-gke1' 
        GKE_PROJECT_ID = '	october-2026-1906' 
        GKE_ZONE = 'us-central1' 
        GIT_REPO_URL = 'https://github.com/kirandevops2026/zomoto-k8s.git' 
        GIT_BRANCH = 'main' 
    }

    stages {
        stage('Checkout Git Repository') {
            steps {
                script {
                    // Checkout the Git repository containing your Kubernetes manifests
                    git url: "${GIT_REPO_URL}", branch: "${GIT_BRANCH}"
                }
            }
        }

        stage('Authenticate with GCP') {
            steps {
                script {
                    // Authenticate with GCP using the service account key
                    sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'
                    sh 'gcloud config set project ${GKE_PROJECT_ID}'
                    sh 'gcloud config set compute/zone ${GKE_ZONE}'
                }
            }
        }

        stage('Configure kubectl') {
            steps {
                script {
                    // Get credentials for the GKE cluster
                    sh 'gcloud container clusters get-credentials ${GKE_CLUSTER_NAME}'
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    // Apply the Kubernetes YAML files to deploy the app
                    sh 'kubectl apply -f aks-store-quickstart.yaml' 
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    // Verify the deployment status
                    sh 'kubectl get pods'
                    sh 'kubectl get svc'
                }
            }
        }
    }
}
