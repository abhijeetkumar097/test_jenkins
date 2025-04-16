pipeline {
    agent any
    
    stages {
        
        stage('Build') {
            steps {
                sh 'docker build -t my_image_name .'
            }
        }

        stage('Run') {
            steps {
                sh 'docker run -d -p 3000:3000 my_image_name'
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
            sh 'docker ps -q | xargs -r docker stop && docker rm $(docker ps -a -q)'
        }
        success {
            echo "Success"
        }
        failure {
            echo "Failure"
        }
    }
}
