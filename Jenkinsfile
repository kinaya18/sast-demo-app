pipeline {
    agent any

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Checkout Code') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/kinaya18/sast-demo-app.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Membuat virtual environment dan mengaktifkannya
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install -r requirements.txt'  // Menggunakan tanda titik (dot) untuk shell
                }
            }
        }

        stage('Run Bandit Scan') {
            steps {
                script {
                    // Menjalankan Bandit untuk Static Application Security Testing (SAST)
                    sh '. venv/bin/activate && bandit -r .'
                }
            }
        }

        stage('Publish Bandit Results') {
            steps {
                script {
                    // Menyimpan hasil scan Bandit ke dalam file
                    sh 'bandit -r . -o bandit_report.json'
                    archiveArtifacts artifacts: 'bandit_report.json', allowEmptyArchive: true
                }
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
