
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'GitHub source code downloaded'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-cicd:${BUILD_NUMBER} .'
            }
        }

        stage('Run Website') {
            steps {
                sh '''
                    docker rm -f jenkins-ci-test 2>/dev/null || true
                    docker run -d --name jenkins-ci-test \
                        -p 127.0.0.1:18080:80 \
                        jenkins-cicd:${BUILD_NUMBER}
                '''
            }
        }

        stage('Test Website') {
            steps {
                sh '''
                    for i in $(seq 1 10); do
                        if curl -fsS http://127.0.0.1:18080/ >/dev/null; then
                            echo "SUCCESS: Website is responding"
                            exit 0
                        fi
                        sleep 2
                    done

                    echo "ERROR: Website did not respond"
                    exit 1
                '''
            }
        }
    }

    post {
        always {
            sh 'docker rm -f jenkins-ci-test 2>/dev/null || true'
        }
    }
}
