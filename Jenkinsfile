pipeline {
    agent any
    stages {
        stage('Build'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v ${WORKSPACE}:/app -w /app'  // Mount workspace and set working directory
                }
            }
            steps{
                sh '''
                echo "Current directory: $(pwd)"
                ls -la
                
                # Check if package.json exists
                if [ ! -f package.json ]; then
                    echo "ERROR: package.json not found!"
                    exit 1
                fi
                
                node --version
                npm --version
                
                # Use --verbose to see more details if npm ci fails
                npm ci --verbose
                npm run build
                
                echo "After build:"
                ls -la
                '''
            }
        }
    }
}