pipeline {
    agent { label "server3" }
    stages {
        stage("checkout code") {
            steps {
                echo "Checking if directory exists"
                sh '''
                    if [ -d "test-application" ]; then
                        echo "Directory exists. Pulling latest changes..."
                        cd test-application
                        git pull
                    else
                        echo "Directory does not exist. Cloning the repository..."
                        git clone https://github.com/aryamanmaurya/test-application.git
                    fi
                '''
                echo "Code update successful"
            }
        }
        stage("build") {
            steps {
                echo "Generating dynamic version for Docker image"

                script {
                    env.VERSION = sh(returnStdout: true, script: 'date +%Y%m%d%H%M%S').trim()
                }


                echo "Building Docker image with version: ${env.VERSION}"
                
                sh '''
                    cd test-application
                    docker build -t test-app:${VERSION} .
                '''

                echo "Build successful with Docker image version: ${env.VERSION}"
            }
        }
    }
}

