pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kinaya18/sast-demo-app.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                script {
                    echo "Setting up virtual environment..."
                    sh "python3 -m venv ${VENV}"
                    sh "./${VENV}/bin/pip install -r requirements.txt"
                }
            }
        }

        stage('SAST Analysis') {
            steps {
                script {
                    echo "Running SAST Analysis with Bandit..."
                    // Run bandit dan abaikan exit code 1 agar tidak menyebabkan build gagal
                    sh "./${VENV}/bin/bandit -r . -f json -o bandit-report.json || true"
                    echo "=== Bandit Report ==="
                    sh "cat bandit-report.json"
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
    }
}
