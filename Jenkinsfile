
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-cicd'
        EC2_HOST = '13.200.243.242'
        EC2_USER = 'ubuntu'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                    docker save ${IMAGE_NAME}:${BUILD_NUMBER} -o website-image.tar
                '''
            }
        }

        stage('Transfer Image to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh-key-v3']) {
                    sh '''
                        scp -o StrictHostKeyChecking=accept-new \
                            website-image.tar \
                            ${EC2_USER}@${EC2_HOST}:/tmp/website-image.tar
                    '''
                }
            }
        }

       
        stage('Deploy to EC2') {
            steps {
                  sshagent(credentials: ['ec2-ssh-key-v3']) {
                    sh """
                      set -e

                     ssh -o StrictHostKeyChecking=accept-new ${EC2_USER}@${EC2_HOST} '
                     set -e
                     docker load -i /tmp/website-image.tar
                     docker rm -f jenkins-cicd-site 2>/dev/null || true
                     docker run -d --restart unless-stopped \
                        --name jenkins-cicd-site \
                        -p 80:80 \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                     rm -f /tmp/website-image.tar
                     '
                    """
                }
            }
        }
 

        stage('Verify Deployment') {
            steps {
                sh '''
                    curl -fsS --retry 5 --retry-delay 3 \
                        http://${EC2_HOST}/ >/dev/null
                    echo "SUCCESS: Website deployed to EC2"
                '''
            }
        }
    }
}
