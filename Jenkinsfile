pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yourdockerhubusername/my-node-app"
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Pull the latest code
                    git 'https://github.com/yourusername/your-repo.git'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Install dependencies
                    sh 'npm install'
                    
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // Run tests using Mocha
                    sh 'npm test'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 3000:3000 $DOCKER_IMAGE'
                }
            }
        }
    }
    
    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        
        success {
            echo 'Pipeline completed successfully!'
        }
        
        failure {
            echo 'Pipeline failed!'
        }
    }
}
