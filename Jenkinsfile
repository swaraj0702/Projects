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
                git branch: 'main', url: 'https://github.com/ahanasrinath/myflaskapp.git'
            }
        }

        stage('Set Up Environment') {
            steps {
                echo 'Setting up virtual environment and installing dependencies...'
                sh '''
                python3 -m venv $VIRTUAL_ENV
                source $VIRTUAL_ENV/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                sh '''
                source $VIRTUAL_ENV/bin/activate
                python -m unittest discover -s . -p "test_*.py"
                '''
            }
        }

        stage('Build and Package') {
            steps {
                echo 'Packaging the application...'
                sh '''
                tar -czf flask-app.tar.gz app.py requirements.txt .env
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sh '''
                # Copy the application to the deployment server
                scp flask-app.tar.gz user@your-server:/path/to/deployment

                # SSH into the server and deploy
                ssh user@your-server << EOF
                cd /path/to/deployment
                tar -xzf flask-app.tar.gz
                source $VIRTUAL_ENV/bin/activate
                nohup python app.py > app.log 2>&1 &
                EOF
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
