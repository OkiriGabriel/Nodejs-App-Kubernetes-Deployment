pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-nodejs-app"  // Name of your Docker image
        IMAGE_TAG = "latest"  // Tag for the image (you could use the commit hash or version)
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from the repository
                checkout scm
            }
        }

        // stage('Install Dependencies') {
        //     steps {
        //         script {
        //             // Install dependencies using npm
        //             sh 'npm install'
        //         }
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image for your Node.js app
                    sh """
                    docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} .
                    """
                }
            }
        }

        // stage('Test Docker Image') {
        //     steps {
        //         script {
        //             // Test Docker image by running it and checking if the app is running
        //             sh """
        //             docker run -d -p 3000:3000 ${DOCKER_IMAGE}:${IMAGE_TAG}
        //             sleep 5  // Wait for the app to start
        //             curl http://localhost:3000  // Test if the app is running
        //             """
        //         }
        //     }
        // }

        stage('Deploy to Server') {
            steps {
                script {
                    // Here, deploy the Docker container to your server
                    // Stop and remove any previous container (if exists)
                    sh """
                    docker stop my-nodejs-app || true
                    docker rm my-nodejs-app || true
                    docker run -d --name my-nodejs-app -p 80:80 ${DOCKER_IMAGE}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up after the build
            sh 'docker system prune -af'
        }

        success {
            echo "Deployment successful!"
        }

        failure {
            echo "Deployment failed!"
        }
    }
}
