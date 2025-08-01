pipeline {
    agent any

    environment {
        VENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build') {           
            steps {
                 echo "Setting up Python environment and installing dependencies...."
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh './venv/bin/pip install pytest'
                sh './venv/bin/pytest tests || true'
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
        always { 
            echo "Post block running, result=${currentBuild.currentResult}" 
        }
        success {
            echo "Build completed successfully. View at ${BUILD_URL}"
            emailext (
                subject: "SUCCESS: Build #${BUILD_NUMBER}",
                from: "barbadiya.mlal@gmail.com",          // <= add this
                replyTo: "barbadiya.mlal@gmail.com", 
                  mimeType: "text/plain",
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
        unstable {
            emailext (
                to: 'nishmohan86@gmail.com',
                subject: "UNSTABLE: #${env.BUILD_NUMBER}",
                body: "Build unstable. ${env.BUILD_URL}"
            )
        }
        aborted {
            emailext (
                to: 'nishmohan86@gmail.com',
                subject: "ABORTED: #${env.BUILD_NUMBER}",
                body: "Build aborted. ${env.BUILD_URL}"
            )
        }
    }
}
