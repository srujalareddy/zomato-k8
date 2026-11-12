pipeline {
    agent any

    environment {
        GKE_CLUSTER_NAME = 'zomot-gke'
        GKE_PROJECT_ID = 'project-00867faf-aa1c-482c-a67'
        GKE_LOCATION = 'us-central1'

        GIT_REPO_URL = 'https://github.com/srujalareddy/zomato-k8.git'
        GIT_BRANCH = 'main'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}",
                    url: "${GIT_REPO_URL}"
            }
        }

        stage('Authenticate using VM Service Account') {
            steps {
                sh '''
                    gcloud auth list
                    gcloud config set project ${GKE_PROJECT_ID}

                    gcloud auth print-access-token >/dev/null
                    echo "VM service account authentication successful"
                '''
            }
        }

        stage('Connect to GKE') {
            steps {
                sh '''
                    gcloud container clusters get-credentials \
                    ${GKE_CLUSTER_NAME} \
                    --zone=${GKE_LOCATION} \
                    --project=${GKE_PROJECT_ID}
                '''
            }
        }

        stage('Verify Cluster') {
            steps {
                sh '''
                    kubectl get nodes
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    kubectl apply -f aks-store-quickstart.yaml
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                    kubectl get deployments
                    kubectl get pods
                    kubectl get services
                '''
            }
        }
    }
}
```