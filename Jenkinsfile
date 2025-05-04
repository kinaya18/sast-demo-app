pipeline {
    agent any

    environment {
        // Misalnya, jika ada environment variable yang perlu didefinisikan
        PYTHON_ENV = 'python3'
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                // Melakukan checkout dari repository GitHub
                checkout scm
            }
        }

        stage('Checkout Code') {
            steps {
                // Pastikan GitHub repository yang benar di-cekout
                git(
                    url: 'https://github.com/kinaya18/sast-demo-app.git', 
                    branch: 'master', 
                    credentialsId: 'github-token'
                )
            }
        }

        stage('Setup Python Environment') {
            steps {
                // Install python dan dependencies yang diperlukan
                script {
                    // Pastikan Python 3 tersedia di agent Jenkins
                    sh 'python3 -m venv venv'
                    sh 'source venv/bin/activate && pip install -r requirements.txt'
                }
            }
        }

        stage('Run Bandit Scan') {
            steps {
                // Jalankan Bandit untuk analisis keamanan kode Python
                script {
                    sh 'source venv/bin/activate && bandit -r . -o bandit_report.html'
                }
            }
        }

        stage('Publish Bandit Results') {
            steps {
                // Menampilkan hasil scan Bandit
                script {
                    // Publish hasil Bandit
                    publishHTML(
                        target: [
                            reportName: 'Bandit Scan Report',
                            reportDir: 'bandit_report.html',
                            reportFiles: 'bandit_report.html',
                            keepAll: false,
                            alwaysLinkToLastBuild: false
                        ]
                    )
                }
            }
        }
    }

    post {
        always {
            // Action yang selalu dijalankan setelah pipeline selesai
            echo 'Pipeline selesai dijalankan.'
        }

        failure {
            // Action yang dijalankan jika pipeline gagal
            echo 'Pipeline gagal. Cek log error untuk info lebih lanjut.'
        }
    }
}
