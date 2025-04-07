pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
        FLASK_PORT = '5000'
    }
    
    stages {
        stage('Check Workspace') {
            steps {
                echo '🔍 Listing workspace contents...'
                bat 'dir /s /b'
            }
        }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main',
                    url: 'https://github.com/swaraj0702/Projects.git',
                    credentialsId: 'github token'
            }
        }

        stage('Set Up Environment') {
            steps {
                echo 'Setting up virtual environment and installing dependencies...'
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\python.exe -m pip install --upgrade pip'
                bat 'venv\\Scripts\\python.exe -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'venv\\Scripts\\python -m pytest > result.log || type result.log'
            }
        }

    stage('Build and Package') {
    steps {
        echo '📦 Packaging app...'
        bat 'if exist dist rmdir /s /q dist'
        bat 'mkdir dist'
        bat 'powershell Compress-Archive -Path app\\* -DestinationPath dist\\app.zip -Force'
    }
}


        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat 'venv\\Scripts\\python app\\app.py'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs for more details.'
        }
    }
}
