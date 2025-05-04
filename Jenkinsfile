pipeline {
    agent any
    
    environment {
        VIRTUAL_ENV = 'venv'
        REQUIREMENTS_FILE = 'requirements.txt'
        BANDIT_REPORT = 'bandit-report.json'
        GIT_REPO_URL = 'https://github.com/kinaya18/sast-demo-app.git' // Pastikan ini URL repositori yang benar
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: "${GIT_REPO_URL}", branch: 'master', credentialsId: 'github-token'  // Ganti kredensial jika diperlukan
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    sh 'python3 -m venv ${VIRTUAL_ENV}'
                    sh """
                        . ${VIRTUAL_ENV}/bin/activate
                        pip install -r ${REQUIREMENTS_FILE}
                    """
                }
            }
        }

        stage('Run Bandit Scan') {
            steps {
                script {
                    sh """
                        . ${VIRTUAL_ENV}/bin/activate
                        bandit -r . -f json -o ${BANDIT_REPORT}
                    """
                }
            }
        }

        stage('Publish Bandit Results') {
            steps {
                script {
                    archiveArtifacts artifacts: "${BANDIT_REPORT}", allowEmptyArchive: true
                }
            }
        }

        stage('Notify') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        echo "Pipeline selesai dengan sukses."
                    } else {
                        echo "Pipeline gagal. Periksa log untuk detail lebih lanjut."
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Membersihkan lingkungan virtual."
            sh 'rm -rf ${VIRTUAL_ENV}'
        }
        failure {
            echo "Pipeline gagal. Periksa log untuk detail lebih lanjut."
        }
        success {
            echo "Pipeline berhasil dijalankan."
        }
    }
}
