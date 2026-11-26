
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
        powershell '''
        $Source = $env:WORKSPACE
        $Destination = "E:\\DjangoProjects"

        Write-Host "Source: $Source"
        Write-Host "Destination: $Destination"

        if (!(Test-Path $Destination)) {
            New-Item -ItemType Directory -Path $Destination -Force
        }

        Get-ChildItem -LiteralPath $Source -Force |
            Where-Object { $_.Name -ne ".git" } |
            ForEach-Object {
                Copy-Item -LiteralPath $_.FullName `
                    -Destination $Destination `
                    -Recurse -Force -ErrorAction Stop
            }

        Write-Host "All application files copied successfully to E drive."
        '''
    }
}
