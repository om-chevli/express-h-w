pipeline {
    agent {
        docker {
            // Use the same Node.js version as in your Dockerfile
            image 'node:7.7-alpine'
            // Specify Dockerfile path relative to Jenkins workspace
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                // Run tests if applicable
                sh 'npm test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build Docker image using Dockerfile
                script {
                    def dockerImage = docker.build('gcr.io/phineasrt/hello-world-image:v1', '--file Dockerfile .')
                    dockerImage.push()
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            environment {
                KUBECONFIG = credentials('your-kubeconfig-credential-id')
            }
            steps {
                // Deploy to Kubernetes using kubectl
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
