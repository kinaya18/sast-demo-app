pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/kinaya18/project-python.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    source $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('Run Bandit Scan') {
            steps {
                sh '''
                    source $VENV_DIR/bin/activate
                    bandit -r . -f html -o bandit-report.html || true
                '''
            }
        }

        stage('Publish Bandit Results') {
            steps {
                archiveArtifacts artifacts: 'bandit-report.html', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline selesai dijalankan.'
        }
        failure {
            echo 'Pipeline gagal. Cek log error untuk info lebih lanjut.'
        }
    }
}
