pipeline {
    agent any

    environment {
        BLUE_TARGET = 'blue'
        GREEN_TARGET = 'green'
        APP_NAME = 'pk'
        NODE_VERSION = '18' // Specify your Node.js version
        BLUE_PORT = 3001
        GREEN_PORT = 3002
    }

    stages {
        stage('Checkout') {
            steps {
                git 'git@github.com:monsad/ec2-blue-green-deployment.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Install dependencies and build your Node.js application
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                // Execute tests for your Node.js application
                // This stage can be expanded based on your testing needs
                sh 'npm test'
            }
        }

        stage('Blue Deployment') {
            steps {
                script {
                    // Deploy to Blue environment
                    sh "pm2 start ${APP_NAME} --name=${BLUE_TARGET} -- --port=${BLUE_PORT}"
                }
            }
        }

        stage('Green Deployment') {
            steps {
                script {
                    // Deploy to Green environment
                    sh "pm2 start ${APP_NAME} --name=${GREEN_TARGET} -- --port=${GREEN_PORT}"
                }
            }
        }

        stage('Swap') {
            steps {
                script {
                    // Route traffic from Blue to Green
                    sh "pm2 delete ${BLUE_TARGET}"
                }
            }
        }

        stage('Post-Deployment Tests') {
            steps {
                // Execute additional tests after deployment
                // This could include smoke tests, integration tests, etc.
            }
        }
    }

    post {
        success {
            echo 'Blue Green Deployment successful!'
        }
        failure {
            echo 'Blue Green Deployment failed!'
            // Optionally trigger rollback mechanism here
        }
    }
}
