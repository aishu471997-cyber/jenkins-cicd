pipeline {
    agent any

    options {
        timestamps()
        skipStagesAfterUnstable()
    }

    environment {
        APP_NAME = 'sample-jenkins-cicd'
        BUILD_VERSION = "1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Starting pipeline for ${APP_NAME}"
                echo "Build number: ${BUILD_NUMBER}"
                echo "Workspace: ${WORKSPACE}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                    . venv/bin/activate
                    python -m pytest -v
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                sh '''
                    mkdir -p dist
                    echo "Application: ${APP_NAME}" > dist/app-info.txt
                    echo "Version: ${BUILD_VERSION}" >> dist/app-info.txt
                    echo "Build date: $(date)" >> dist/app-info.txt
                    tar -czf ${APP_NAME}-${BUILD_VERSION}.tar.gz app.py test_app.py dist
                '''
                archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Simulating deployment...'
                sh '''
                    echo "Deployed ${APP_NAME} version ${BUILD_VERSION} to staging environment"
                '''
            }
        }
    }

    post {
        success {
            echo 'PIPELINE SUCCESS: Build, test, package, and simulated deployment completed.'
        }
        failure {
            echo 'PIPELINE FAILED: Check the console output above for the failing stage.'
        }
        always {
            echo 'Cleaning up pipeline workspace.'
            cleanWs()
        }
    }
}
