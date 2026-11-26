
pipeline {
    agent any

    stages {

        stage('1. Git Checkout') {
            steps {
                echo 'Downloading source code from GitHub'

                git branch: 'main',
                    url: 'https://github.com/mohanprasadme/Django-Simple-Login.git'
            }
        }

        stage('2. Copy Files to E Drive') {
            steps {
                bat '''
                echo Creating deployment directory...

                if not exist "E:\\DjangoProjects" (
                    mkdir "E:\\DjangoProjects"
                )

                echo Copying GitHub repository files...

                robocopy "%WORKSPACE%" "E:\\DjangoProjects" /E /COPY:DAT /R:2 /W:2 /XD .git

                if %ERRORLEVEL% GEQ 8 exit /b 1

                echo Files copied successfully to E:\\DjangoProjects
                '''
            }
        }

        stage('3. Create Virtual Environment') {
            steps {
                bat '''
                cd /d E:\\DjangoProjects

                if not exist "venv\\Scripts\\python.exe" (
                    python -m venv venv
                )
                '''
            }
        }

        stage('4. Install Dependencies') {
            steps {
                bat '''
                cd /d E:\\DjangoProjects

                venv\\Scripts\\python.exe -m pip install --upgrade pip

                venv\\Scripts\\python.exe -m pip install -r requirements.txt
                '''
            }
        }

        stage('5. Django Migration') {
            steps {
                bat '''
                cd /d E:\\DjangoProjects

                venv\\Scripts\\python.exe manage.py migrate
                '''
            }
        }

        stage('6. Django System Check') {
            steps {
                bat '''
                cd /d E:\\DjangoProjects

                venv\\Scripts\\python.exe manage.py check
                '''
            }
        }
    }

    post {
        success {
            echo 'Django application deployment completed successfully!'
            echo 'Application location: E:\\DjangoProjects'
        }

        failure {
            echo 'Deployment failed. Check the Jenkins Console Output.'
        }
    }
}
