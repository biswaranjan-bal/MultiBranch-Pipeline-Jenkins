pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3.6.3'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out code from branch: ${env.GIT_BRANCH}"
                }
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn clean compile'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running unit tests..."
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    echo "Packaging the application..."
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Archive') {
            steps {
                script {
                    echo "Archiving artifacts..."
                    archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed!"
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
        success {
            echo "Build successful!"
        }
        failure {
            echo "Build failed! Check the logs for details."
        }
    }
}
