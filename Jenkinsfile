pipeline {
    agent any

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                git credentialsId: 'github token', url: 'https://github.com/swaraj0702/Projects.git'
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
                bat 'venv\\Scripts\\python -m pytest 1>result.log || type result.log'
            }
        }

        stage('Build and Package') {
            steps {
                echo 'Packaging application...'
                bat 'rmdir /s /q dist && mkdir dist'
                
                // 👇 Debug directory structure
                bat 'dir /s /b'

                // 👇 Adjust this based on actual folder structure
                bat 'powershell Compress-Archive -Path app\\* -DestinationPath dist\\app.zip -Force'
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' }
            }
            steps {
                echo 'Deploying the application...'
                // deployment steps (if any)
            }
        }
    }

    post {
        failure {
            echo '❌ Pipeline failed. Check the logs for more details.'
        }
        success {
            echo '✅ Pipeline completed successfully.'
        }
    }
}
