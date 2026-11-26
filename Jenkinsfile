
stage('1. Git Checkout') {
    steps {
        git branch: 'main',
            url: 'https://github.com/mohanprasadme/Django-Simple-Login.git'
    }
}

stage('2. Copy Files to E Drive') {
    steps {
        bat '''
        if not exist "E:\\DjangoProjects" mkdir "E:\\DjangoProjects"

        robocopy "%WORKSPACE%" "E:\\DjangoProjects" /E /COPY:DAT /R:2 /W:2 /XD .git

        if %ERRORLEVEL% GEQ 8 exit /b 1

        echo All GitHub repository files copied to E:\\DjangoProjects successfully.
        '''
    }
}


        
