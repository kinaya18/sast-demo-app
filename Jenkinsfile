pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone repo dari GitHub Kinaya
                git 'https://github.com/kinaya18/project-python.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                // Install virtualenv dan dependencies
                sh '''
                    python3 -m venv ${VENV_DIR}
                    source ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install bandit -r requirements.txt
                '''
            }
        }

        stage('Run Bandit Scan') {
            steps {
                // Jalankan bandit scan
                sh '''
                    source ${VENV_DIR}/bin/activate
                    bandit -r . -f json -o bandit-report.json || true
                '''
            }
        }

        stage('Publish Bandit Results') {
            steps {
                // Tampilkan hasil scan
                sh 'cat bandit-report.json'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'bandit-report.json', fingerprint: true
        }
    }
}
