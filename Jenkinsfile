pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        REQUIREMENTS_FILE = 'requirements.txt'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Menarik kode dari repositori GitHub
                checkout scm
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                // Membuat virtual environment jika belum ada
                script {
                    if (!fileExists(VENV_DIR)) {
                        sh "python3 -m venv ${VENV_DIR}"
                    }
                }
                // Menginstal dependensi dari requirements.txt
                sh './venv/bin/pip install -r ${REQUIREMENTS_FILE}'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Menginstal Bandit untuk analisis SAST
                sh './venv/bin/pip install bandit'
            }
        }

        stage('SAST Analysis') {
            steps {
                // Menjalankan analisis menggunakan Bandit
                sh './venv/bin/bandit -r . -f json -o bandit_report.json'
            }
        }

        stage('Post Actions') {
            steps {
                // Menghapus virtual environment setelah analisis selesai
                script {
                    if (fileExists(VENV_DIR)) {
                        sh "rm -rf ${VENV_DIR}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Menampilkan laporan Bandit setelah pipeline selesai
            archiveArtifacts artifacts: 'bandit_report.json', allowEmptyArchive: true
            // Membersihkan workspace
            cleanWs()
        }

        success {
            echo 'Pipeline berhasil dijalankan!'
        }

        failure {
            echo 'Pipeline gagal dijalankan!'
        }
    }
}
