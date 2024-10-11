pipeline {
    agent any
    stages {
        stage('Info') {
            steps {
                script {
                    // Print branch name and build ID
                    echo "Branch Name: ${env.GIT_BRANCH}"
                    echo "Build ID: ${env.BUILD_ID}"
                }
            }
        }
        stage('Build') {
            steps {
                echo "This is Build stage"
            }
        }

        stage('Test') {
            steps {
                echo "This is Test stage"
            }
        }
        stage('Deploy') {
            steps {
                echo "This is Deploy stage"
            }
        }
    }
}
