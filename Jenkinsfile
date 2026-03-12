pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/Priyanshu-96/NextGen-Medicine-Recommendation.git"
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
                dir('NextGen-Medicine-Recommendation') {
                    sh 'npm install'
                }
            }
        }


        stage('Build Project') {
            steps {
                echo "Building project..."
                dir('NextGen-Medicine-Recommendation') {
                    sh 'npm run build'
                }
            }
        }

        stage('Verify Build Folder') {
            steps {
                dir('NextGen-Medicine-Recommendation') {
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
