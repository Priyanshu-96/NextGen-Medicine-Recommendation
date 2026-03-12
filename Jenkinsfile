pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/Priyanshu-96/NextGen-Medicine-Recommendation.git"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                sh 'rm -rf *'
            }
        }

        stage('Clone Repository') {
            steps {
                sh 'git clone $REPO_URL .'
            }
        }

        stage('Check System Environment') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'python3 --version'
            }
        }

        stage('Frontend Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Frontend Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Verify Frontend Build') {
            steps {
                sh '''
                if [ ! -d "build" ]; then
                    echo "Frontend build folder missing"
                    exit 1
                fi
                '''
            }
        }

        stage('Backend Validation') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    sh 'node -c server.js'
                }
            }
        }

        stage('Python Service Validation') {
            steps {
                dir('python-microservice') {
                    sh 'pip3 install -r requirements.txt'
                    sh 'python3 -m py_compile main.py'
                }
            }
        }

        stage('Dataset Check') {
            steps {
                sh '''
                if [ ! -d "datasets" ]; then
                    echo "Datasets folder missing"
                    exit 1
                fi
                '''
            }
        }

    }

    post {
        success {
            echo "CI Pipeline Passed"
        }
        failure {
            echo "CI Pipeline Failed"
        }
        always {
            echo "Pipeline Finished"
        }
    }
}
