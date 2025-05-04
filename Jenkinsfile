pipeline {
    agent any

    environment {
        VENV = 'venv'  // Nama virtual environment
    }

    stages {
        // Stage pertama: Checkout kode dari GitHub
        stage('Checkout') {
            steps {
                git 'https://github.com/kinaya18/sast-demo-app.git'
            }
        }

        // Stage kedua: Setup Virtual Environment
        stage('Setup Virtual Environment') {
            steps {
                script {
                    // Cek jika sudah ada virtual environment atau perlu dibuat baru
                    echo "Setting up virtual environment..."
                    sh 'python3 -m venv ${VENV}'
                    sh './${VENV}/bin/pip install -r requirements.txt'  // Install dependencies
                }
            }
        }

        // Stage ketiga: Jalankan analisis SAST menggunakan Bandit
        stage('SAST Analysis') {
            steps {
                script {
                    echo "Running SAST Analysis with Bandit..."
                    
                    // Jalankan Bandit untuk analisis keamanan
                    sh './${VENV}/bin/bandit -r . -f json -o bandit-report.json'
                    
                    // Menampilkan laporan Bandit di terminal (opsional)
                    sh 'cat bandit-report.json'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()  // Menghapus workspace setelah pipeline selesai
        }
    }
}
