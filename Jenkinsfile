pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kinaya18/sast-demo-app.git'
            }
        }
        
        stage('Setup Virtual Environment') {
            steps {
                script {
                    // Membuat virtual environment jika diperlukan
                    echo "Setting up virtual environment"
                    sh 'python3 -m venv venv'
                    sh './venv/bin/pip install bandit'  // Install Bandit dalam virtual environment
                }
            }
        }

        stage('SAST Analysis') {
            steps {
                script {
                    // Menjalankan Bandit untuk analisis SAST pada aplikasi
                    echo "Running SAST Analysis with Bandit..."
                    sh './venv/bin/bandit -r . -o bandit-report.html'  // Analisis dengan Bandit dan hasilkan laporan HTML
                }
            }
        }
    }
    
    post {
        always {
            echo "Cleaning up workspace"
            cleanWs()  // Menghapus workspace setelah pipeline selesai
        }
    }
}
