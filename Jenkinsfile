pipeline {
    agent {
        docker {
            // image 'node:16-buster-slim': Parameter image ini mengunduh Docker image bernama node:16-buster-slim dan menjalankan image sebagai container terpisah. 
            // - Anda akan memiliki Jenkins container dan Node container terpisah yang berjalan secara lokal di Docker. 
            // - Node container menjadi agent yang digunakan Jenkins untuk menjalankan Pipeline project Anda. Namun, container ini hanya akan berjalan selama durasi eksekusi Pipeline Anda saja.
            // - args '-p 3000:3000': Parameter args ini membuat Node container ini dapat diakses (sementara) melalui port 3000. Ini penting untuk menjalankan berkas jenkins/scripts/deliver.sh. 
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                // Increase npm Fetch Timeout 
                // The default timeout for npm might be too short for the container's virtualized network. 
                // You can increase it by setting the fetch-retries and fetch-retry-maxtimeout in your pipeline script or an .npmrc file.
                sh 'npm config set fetch-retries 5'
                sh 'npm config set fetch-retry-mintimeout 20000'
                sh 'npm config set fetch-retry-maxtimeout 120000'
                sh 'npm install --verbose'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}