pipeline {
    agent any
    stages {
        /*
        stage('Build'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v ${WORKSPACE}:/app -w /app'
                }
            }
            steps{
                sh '''
                # Set npm cache directory to a location with write permissions
                export HOME=${WORKSPACE}
                export npm_config_cache=${WORKSPACE}/.npm
                
                ls -la
                node --version
                npm --version
                
                # Use npm install instead of npm ci
                npm install
                npm run build
                ls -la
                '''
            }
        }
        */
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v ${WORKSPACE}:/app -w /app'
                }
            }
            steps{
                sh '''
                    # Check if index.html exists in the build folder
                    if [ -f build/index.html ]; then
                        echo "✅ index.html found in build folder"
                    else
                        echo "❌ ERROR: index.html not found in build folder"
                    fi

                    echo "Running tests..."
                    npm test
                '''
                
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.52.0-noble'
                    reuseNode true
                    args '-v ${WORKSPACE}:/app -w /app'
                }
            }
            steps{
                sh '''
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
                    source ~/.bashrc
                    nvm install node
                    npm install -g serve
                    sleep 10
                    npx playwrite test
                '''
            }
        }
    }

    post{
        always {
            junit 'test-results/'
        }
    }
}