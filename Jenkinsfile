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
                sh 'git clone $REPO_URL'
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
                dir('NextGen-Medicine-Recommendation') {
                    sh 'npm install'
                }   
            }
        }

        stage('Frontend Build') {
            steps {
                dir('NextGen-Medicine-Recommendation') {
                    sh 'CI=false npm run build'
                }
            }
        }

        stage('Verify Frontend Build') {
            steps {
                dir('NextGen-Medicine-Recommendation') {
                    sh '''
                    if [ ! -d "build" ]; then
                        echo "Frontend build folder missing"
                        exit 1
                    fi
                    '''
                }
            }
        }

        stage('Backend Validation') {
            steps {
                dir('NextGen-Medicine-Recommendation/backend') {
                    sh 'npm install'
                    sh 'node -c index.js'
                }
            }
        }

        stage('Python Service Validation') {
            steps {
                dir('NextGen-Medicine-Recommendation/python_microservice') {
                    sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                    cd app
                    python -m py_compileall app
                    '''
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
