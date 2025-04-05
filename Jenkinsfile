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
                %VIRTUAL_ENV%\\Scripts\\python.exe -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat '''
                call %VIRTUAL_ENV%\\Scripts\\activate
                REM You can switch between unittest and pytest by commenting one of the following lines:

                REM For unittest - make sure tests are in the root or adjust `-s` and `-p`
                REM python -m unittest discover -s tests -p "test_*.py"

                REM For pytest
                pytest
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
                bat '''
                echo Simulating deployment...
                REM Example: xcopy /Y /I flask-app.zip \\\\your-server\\deployment\\
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
