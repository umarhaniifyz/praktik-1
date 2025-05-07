pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\Hanif\\AppData\\Local\\Programs\\Python\\Python310\\python.exe'
        PIP = 'C:\\Users\\Hanif\\AppData\\Local\\Programs\\Python\\Python310\\Scripts\\pip.exe'
    }

    stages {
        stage('Verify Python & pip') {
            steps {
                bat '''
                    echo Checking Python and pip...
                    "%PYTHON%" --version
                    "%PIP%" --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                    echo Installing dependencies...
                    if exist requirements.txt (
                        "%PIP%" install -r requirements.txt
                    ) else (
                        echo requirements.txt NOT FOUND!
                        exit /b 1
                    )
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                    echo Running tests...
                    "%PYTHON%" -m pytest test_app.py
                '''
            }
        }
    }

    post {
        success {
            script {
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson([
                        content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]),
                    url: 'https://discord.com/api/webhooks/1369578608624144384/tXq_cJqQaGsxMLrYLhe-f3Qkik5owCB-_5bGtyj6lq0Me0Zzkw6Zijtt39-7FQ7bvdRa'
                )
            }
        }

        failure {
            script {
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson([
                        content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]),
                    url: 'https://discord.com/api/webhooks/1369578608624144384/tXq_cJqQaGsxMLrYLhe-f3Qkik5owCB-_5bGtyj6lq0Me0Zzkw6Zijtt39-7FQ7bvdRa'
                )
            }
        }
    }
}
