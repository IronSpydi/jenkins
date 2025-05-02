pipeline {
    agent any
    stages {
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
        stage('Test'){
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
    }
}