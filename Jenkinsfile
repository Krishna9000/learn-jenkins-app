pipeline {
    agent any

    stages {
        // This is a comment
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
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                if ls build | grep -q 'index.html'; then
                    echo "File exists"
                else
                    echo "File does not exist"
                    exit 1
                fi
                npm test
                '''
            }
        }
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 20
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}