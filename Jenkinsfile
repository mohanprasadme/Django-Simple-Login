
pipeline {
    agent any

    stages {

        stage('1. Git Checkout') {
            steps {
                echo 'Downloading application from GitHub'

                git branch: 'main',
                    url: 'https://github.com/mohanprasadme/Django-Simple-Login.git'
            }
        }

        stage('2. Copy Files to E Drive') {
            steps {
                powershell '''
                    $Source = $env:WORKSPACE
                    $Destination = "E:\\DjangoProjects"

                    Write-Host "Source: $Source"
                    Write-Host "Destination: $Destination"

                    if (!(Test-Path -LiteralPath $Destination)) {
                        New-Item -ItemType Directory -Path $Destination -Force
                    }

                    Get-ChildItem -LiteralPath $Source -Force |
                        Where-Object { $_.Name -ne ".git" } |
                        ForEach-Object {
                            Copy-Item -LiteralPath $_.FullName `
                                -Destination $Destination `
                                -Recurse -Force -ErrorAction Stop
                        }

                    Write-Host "Files copied successfully to E drive."
                '''
            }
        }

        stage('3. Create Python Virtual Environment') {
            steps {
                bat '''
                    cd /d E:\\DjangoProjects

                    if not exist venv\\Scripts\\python.exe (
                        python -m venv venv
                    )
                '''
            }
        }

        stage('4. Install Requirements') {
            steps {
                bat '''
                    cd /d E:\\DjangoProjects

                    venv\\Scripts\\python.exe -m pip install --upgrade pip

                    venv\\Scripts\\python.exe -m pip install -r requirements.txt
                '''
            }
        }

        stage('5. Django Database Migration') {
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
            echo 'Django deployment completed successfully!'
            echo 'Deployment location: E:\\DjangoProjects'
        }

        failure {
            echo 'Deployment failed. Please check Console Output.'
        }
    }
}

