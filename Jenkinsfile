pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout kode dari repository Git
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Membuat virtual environment
                    sh 'python3 -m venv venv'
                    
                    // Mengaktifkan virtual environment dan menginstal bandit
                    sh './venv/bin/pip install --upgrade pip'
                    sh './venv/bin/pip install bandit'
                }
            }
        }
        stage('SAST Analysis') {
            steps {
                script {
                    // Menjalankan Bandit untuk analisis SAST
                    sh './venv/bin/bandit -r .'
                }
            }
        }
    }
    post {
        always {
            // Membersihkan virtual environment setelah pipeline selesai
            sh 'rm -rf venv'
        }
    }
}
