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
    }
}