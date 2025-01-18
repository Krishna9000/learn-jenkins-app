pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                ls -al
                node --version
                npm --version
                npm ci
                npm run build
                ls -al
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                if ls build | grep -q 'build\index.html'; then
                    echo "File exists"
                else
                    echo "File does not exist"
                    exit 1
                fi
                npm test
                '''
            }
        }
    }
}