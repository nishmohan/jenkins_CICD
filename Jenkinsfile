pipeline {
    agent any

    environment {
        VENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build') {           
            steps {
                 echo "Setting up Python environment and installing dependencies...."
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate'
                sh './$VENV/bin/pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh './$VENV/bin/pip install pytest'
                sh './$VENV/bin/pytest tests || true'
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Deploying Flask app to staging...'
                sh 'nohup ./$VENV/bin/python app.py &'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: Build #${BUILD_NUMBER}",
                body: "Build completed successfully. View at ${BUILD_URL}",
                to: "nishmohan86@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "FAILURE: Build #${BUILD_NUMBER}",
                body: "Build failed. View logs at ${BUILD_URL}",
                to: "nishmohan86@gmail.com"
            )
        }
    }
}
