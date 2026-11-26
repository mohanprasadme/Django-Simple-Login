
pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'E:\\DjangoProjects\\SimpleLogin'
        SERVICE_NAME = 'SimpleLogin'
    }

    stages {

    stage('1. Git Checkout') {
        steps {
            echo 'Checking out source code from GitHub'

            git branch: 'main',
                url: 'https://github.com/mohanprasadme/Django-Simple-Login.git'
        }
    }
}
        stage('2. Copy Files to E Drive') {
            steps {
                bat '''
                if not exist "%DEPLOY_DIR%" mkdir "%DEPLOY_DIR%"

                robocopy . "%DEPLOY_DIR%" /E /XD .git venv __pycache__ /XF db.sqlite3

                if errorlevel 8 exit /b 1
                '''
            }
        }

        stage('3. Create Virtual Environment') {
            steps {
                bat '''
                if not exist "%DEPLOY_DIR%\\venv\\Scripts\\python.exe" (
                    python -m venv "%DEPLOY_DIR%\\venv"
                )
                '''
            }
        }

        stage('4. Install Dependencies') {
            steps {
                bat '''
                "%DEPLOY_DIR%\\venv\\Scripts\\python.exe" -m pip install --upgrade pip

                "%DEPLOY_DIR%\\venv\\Scripts\\python.exe" -m pip install -r "%DEPLOY_DIR%\\requirements.txt"
                '''
            }
        }

        stage('5. Django Migration') {
            steps {
                bat '''
                cd /d "%DEPLOY_DIR%"

                "%DEPLOY_DIR%\\venv\\Scripts\\python.exe" manage.py migrate
                '''
            }
        }

        stage('6. Django Validation') {
            steps {
                bat '''
                cd /d "%DEPLOY_DIR%"

                "%DEPLOY_DIR%\\venv\\Scripts\\python.exe" manage.py check
                '''
            }
        }

        stage('7. Restart Application') {
            steps {
                bat '''
                sc query "%SERVICE_NAME%" >nul 2>&1

                if not errorlevel 1 (
                    net stop "%SERVICE_NAME%"
                    net start "%SERVICE_NAME%"
                ) else (
                    echo Windows service not installed yet.
                    echo Install the service after the first deployment.
                )
                '''
            }
        }
    }

    post {
        success {
            echo 'Django deployment completed successfully!'
        }

        failure {
            echo 'Django deployment failed. Check Jenkins Console Output.'
        }
    }
}
