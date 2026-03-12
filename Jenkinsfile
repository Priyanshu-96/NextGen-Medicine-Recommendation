pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/Priyanshu-96/Personal-Portfolio.git"
    }

    stages {

        stage('Clear Worspace') {
            steps {
                sh 'rm -Rf *'
            }
        }

        stage('Clone Repo') {
            steps {
                sh 'git clone $REPO_URL'
            }
        }

        stage('Check Node Env') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                dir('Personal-Portfolio') {
                    sh 'npm install'
                }
            }
        }


        stage('Build Project') {
            steps {
                echo "Building project..."
                dir('Personal-Portfolio/portfolio-vite') {
                    sh 'npm install typescript'
                    sh 'npm run build'
                }
            }
        }

        stage('Verify Build Folder') {
            steps {
                dir('Personal-Portfolio/portfolio-vite') {
                    sh '''
                    if [ ! -d "dist" ]; then
                        echo "Build folder not found. Build failed."
                        exit 1
                    fi
                    '''
                }
            }
        }
        stage('Workspace Info') {
            steps {
                echo "THIS STAGE IS RUNNING"
                sh 'pwd'
                sh 'ls -la'
            }
        }
    }

    post {
        success {
            echo "Build Successful ✅"
        }
        failure {
            echo "Build Failed ❌"
        }
        always {
            echo "Pipeline Finished"
        }
    }
}
