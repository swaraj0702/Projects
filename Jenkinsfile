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
        echo 'Packaging application...'
        bat 'mkdir dist'
        bat 'powershell Compress-Archive -Path Projects\\* -DestinationPath dist\\app.zip -Force'

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
