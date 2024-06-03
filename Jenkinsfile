pipeline {
    agent any

    environment {
        TESTING_ENVIRONMENT = "Automated"
        PRODUCTION_ENVIRONMENT = "Ashwini"
        DOCKER_IMAGE = "yourdockerhubusername/my-node-app"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Fetching the source code...'
                    // Pull the latest code with credentials
                    git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/Ashdeore/NewDevopspipeline.git'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    // Install dependencies
                    sh 'npm install'
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Run tests using Mocha
                    sh 'npm test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Checking the quality of the code...'
                    // Run code quality analysis, e.g., with ESLint
                    sh 'npm run lint'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing a security scan on the code...'
                    // Run security scan, e.g., with npm audit
                    sh 'npm audit'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying the application to a staging server...'
                    // Run the Docker container in a staging environment
                    sh 'docker run -d -p 3000:3000 --name staging_app $DOCKER_IMAGE'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on the staging environment...'
                    // Run integration tests on staging environment
                    // Placeholder command for actual integration tests
                    sh 'curl -f http://localhost:3000 || exit 1'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying the application to a production server...'
                    // Remove staging container before deploying to production
                    sh 'docker rm -f staging_app || true'
                    // Run the Docker container in a production environment
                    sh 'docker run -d -p 80:3000 --name production_app $DOCKER_IMAGE'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up workspace after build
                cleanWs()
                // Send email notification
                emailext(
                    body: "Pipeline finished with status: ${currentBuild.result}",
                    subject: "Pipeline ${currentBuild.result}: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    to: "ashwinideore2704@gmail.com",
                    attachLog: true
                )
            }
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
