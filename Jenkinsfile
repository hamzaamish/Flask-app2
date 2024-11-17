pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'your-github-credentials-id', url: 'https://github.com/<your-repo>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-flask-app:latest')
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-ssh-credentials-id']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@<ec2-public-ip> <<EOF
                    docker stop my-flask-app || true
                    docker rm my-flask-app || true
                    docker run -d --name my-flask-app -p 5000:5000 my-flask-app:latest
                    EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully on EC2!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

