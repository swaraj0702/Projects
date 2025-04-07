pipeline {
    agent any

    environment {
        FLASK_APP = 'app.py'
        FLASK_PORT = 5000
    }

    stages {
        stage('Check Workspace') {
            steps {
                echo '🔍 Listing workspace contents...'
                bat 'dir /s /b'
            }
        }

        stage('Clone Repository') {
            steps {
                echo '📥 Cloning the repository...'
                git branch: 'main',
                    url: 'https://github.com/swaraj0702/Projects.git',
                    credentialsId: 'github token'
            }
        }

        stage('Install Requirements') {
            steps {
                echo '📦 Installing requirements...'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Package App') {
            steps {
                echo '📁 Creating dist directory and zipping app...'
                bat '''
                if not exist dist mkdir dist
                powershell Compress-Archive -Path Projects\\app\\* -DestinationPath dist\\app.zip -Force
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo '🚀 Running the Flask app...'
                bat 'python Projects\\app\\app.py'
            }
        }
    }
}
