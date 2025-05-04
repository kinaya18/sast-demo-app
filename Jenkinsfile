pipeline { 
    agent any 
 
    stages { 
        stage('Checkout') { 
            steps { 
                git 'https://github.com/kinaya18/sast-demo-app.git' 
            } 
        } 
        
        stage('Install Dependencies') { 
            steps { 
                // Menggunakan virtual environment
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install bandit'
            }
        }
        
        stage('SAST Analysis') { 
            steps { 
                sh './venv/bin/bandit -f xml -o bandit-output.xml -r . || true' 
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')] 
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
