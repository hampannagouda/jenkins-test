pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        DEPLOY_DIR = 'C:\\jenkins-deploy\\site'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                echo 'Checking index.html...'

                bat '''
                    if not exist index.html (
                        echo ERROR: index.html not found
                        exit /b 1
                    )

                    echo index.html found successfully
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website...'

                bat '''
                    if not exist "%DEPLOY_DIR%" mkdir "%DEPLOY_DIR%"

                    robocopy . "%DEPLOY_DIR%" /MIR /XD .git /XF Jenkinsfile

                    if %ERRORLEVEL% GEQ 
