pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv' // Virtual environment directory
        FLASK_PORT = '5000' // Port to run the Flask app
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
                bat '''
                python -m venv %VIRTUAL_ENV%
                call %VIRTUAL_ENV%\\Scripts\\activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat '''
                call %VIRTUAL_ENV%\\Scripts\\activate
                python -m unittest discover -s . -p "test_*.py"
                '''
            }
        }

        stage('Build and Package') {
            steps {
                echo 'Packaging the application...'
                bat '''
                powershell Compress-Archive -Path app.py,requirements.txt,.env -DestinationPath flask-app.zip
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Replace with real deployment logic for Windows — could be file copy or some custom script
                bat '''
                echo Simulating deployment...
                REM xcopy /Y /I flask-app.zip \\\\your-server\\deployment\\
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
