pipeline {
    agent any

    environment {
        TARGET_DIR = 'D:/piplinejenkinsWebsite'  // IIS deployment folder
        APP_POOL = 'jenikinspipe'        // IIS App Pool Name
        SITE_NAME = 'jenikinspipe'       // IIS Site Name (optional)
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning from GitHub...'
                checkout scm  // Checkout the repository from GitHub (as configured in Jenkins)
            }
        }

        stage('Stop IIS App Pool') {
            steps {
                echo "🛑 Stopping IIS App Pool: ${env.APP_POOL}..."
                bat "powershell.exe Stop-WebAppPool -Name '${env.APP_POOL}'"
            }
        }

        stage('Copy to IIS') {
            steps {
                echo "🚀 Deploying static files to IIS folder: ${env.TARGET_DIR}..."
                bat """
                    if not exist "${env.TARGET_DIR}" mkdir "${env.TARGET_DIR}"
                    xcopy /s /y /i "*.*" "${env.TARGET_DIR}\\"
                """
            }
        }

        stage('Start IIS App Pool') {
            steps {
                echo "▶️ Starting IIS App Pool: ${env.APP_POOL}..."
                bat "powershell.exe Start-WebAppPool -Name '${env.APP_POOL}'"
            }
        }

        stage('Done') {
            steps {
                echo '🎉 Deployment complete on IIS server.'
            }
        }
    }
}
