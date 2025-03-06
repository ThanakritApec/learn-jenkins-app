pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'f561d6c4-55b4-40a8-99db-d284ea24aafc'
        NETLIFY_AUTH = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "================Building the project================"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }


        stage('Test')   {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    echo "================Testing the project================"
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "================Deploying the project================"
                    echo "Deploying to Netlify Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    

                '''
            }
        }


    }
    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}
