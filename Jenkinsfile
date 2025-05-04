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
                    // Jika tidak ada dependencies, tidak perlu virtual environment
                    echo "No dependencies to install"
                }
            }
        }

        stage('SAST Analysis') {
            steps {
                // Langkah-langkah analisis SAST di sini
                echo "Running SAST Analysis..."
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
