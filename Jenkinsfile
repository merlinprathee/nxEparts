pipeline {
    agent any

    environment {
        IIS_DIR_ADMIN  = "C:\\inetpub\\wwwroot\\admin"
        IIS_DIR_BUYER  = "C:\\inetpub\\wwwroot\\buyer"
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
                bat "npx nx build admin --configuration=production"
                bat "npx nx build buyer --configuration=production"
                bat "npx nx build seller --configuration=production"
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    // Admin
                    bat "if exist %IIS_DIR_ADMIN% rmdir /S /Q %IIS_DIR_ADMIN%"
                    bat "mkdir %IIS_DIR_ADMIN%"
                    bat "xcopy dist\\apps\\admin\\* %IIS_DIR_ADMIN% /E /H /C /I"

                    // Buyer
                    bat "if exist %IIS_DIR_BUYER% rmdir /S /Q %IIS_DIR_BUYER%"
                    bat "mkdir %IIS_DIR_BUYER%"
                    bat "xcopy dist\\apps\\buyer\\* %IIS_DIR_BUYER% /E /H /C /I"

                    // Seller
                    bat "if exist %IIS_DIR_SELLER% rmdir /S /Q %IIS_DIR_SELLER%"
                    bat "mkdir %IIS_DIR_SELLER%"
                    bat "xcopy dist\\apps\\seller\\* %IIS_DIR_SELLER% /E /H /C /I"
                }
            }
        }
    }
}
