pipeline {
    agent any

    environment {
        gitUrl = 'https://github.com/abhijeetkumar097/test_jenkins.git'
    }

    stages {
        stage("Git Clone") {
            steps {
                git branch: 'main', url: "${gitUrl}"
            }
        }

        stage('Build') {
            steps {
                sh "docker compose build --no-cache"
            }
        }

        stage('Run') {
            steps {
                sh "docker compose up -d"
            }
        }

        stage('Health check') {
            steps {
                script {
                    retry(3) {
                        sh "sleep 5"
                        sh "curl http://localhost:3000/"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Shutting down"
            sh "docker compose down"
        }
        success {
            echo "Success"
        }
        failure {
            echo "Failure"
        }
    }
}
