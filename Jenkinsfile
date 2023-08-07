pipeline {
    agent any
    
    environment {
        NEXUS_REPO_URL = "http://localhost:8081/repository"  // Replace with your actual Nexus Repository URL
        NEXUS_REPO_NAME = "vueJs-Dockerized"               // Replace with your actual Nexus Repository Name
        DOCKER_IMAGE_TAG = "vueJs-app:latest"
    }
    
    stages {
        stage('Build') {
            steps {
                // Build the Vue.js app in a Docker container
                sh 'docker build -t $DOCKER_IMAGE_TAG .'
            }
        }
        
        stage('Push to Nexus') {
            steps {
                // Authenticate with your Nexus repository using credentials
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials-id', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USERNAME')]) {
                    // Tag the Docker image with the Nexus repository URL and push it
                    sh "docker tag $DOCKER_IMAGE_TAG $NEXUS_REPO_URL/$NEXUS_REPO_NAME/$DOCKER_IMAGE_TAG"
                    sh "docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_REPO_URL"
                    sh "docker push $NEXUS_REPO_URL/$NEXUS_REPO_NAME/$DOCKER_IMAGE_TAG"
                }
            }
        }
    }
}
