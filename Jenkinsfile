pipeline {
    agent any
    // tools {
    //     nodejs "node22" // configured in Jenkins global tools
    // }

    environment {
        IIS_DIR_ADMIN = "C:\\inetpub\\wwwroot\\admin"
        IIS_DIR_BUYER = "C:\\inetpub\\wwwroot\\buyer"
        IIS_DIR_SELLER = "C:\\inetpub\\wwwroot\\seller"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/merlinprathee/nxEparts.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat "npm install"
            }
        }

        stage('Build Projects') {
            steps {
                bat "npx nx build admin --prod"
                bat "npx nx build buyer --prod"
                bat "npx nx build seller --prod"
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    bat "if exist %IIS_DIR_ADMIN% rmdir /S /Q %IIS_DIR_ADMIN%"
                    bat "mkdir %IIS_DIR_ADMIN%"
                    bat "xcopy apps\\admin\\dist\\admin\\* %IIS_DIR_ADMIN% /E /H /C /I"

                    bat "if exist %IIS_DIR_BUYER% rmdir /S /Q %IIS_DIR_BUYER%"
                    bat "mkdir %IIS_DIR_BUYER%"
                    bat "xcopy apps\\buyer\\dist\\buyer\\* %IIS_DIR_BUYER% /E /H /C /I"

                    bat "if exist %IIS_DIR_SELLER% rmdir /S /Q %IIS_DIR_SELLER%"
                    bat "mkdir %IIS_DIR_SELLER%"
                    bat "xcopy apps\\seller\\dist\\seller\\* %IIS_DIR_SELLER% /E /H /C /I"
                }
            }
        }
    }
}
