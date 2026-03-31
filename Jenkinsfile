pipeline {
    
    agent any

    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
    }

    stages {
        stage('Checkout'){
            steps {
                checkout scm
            }
        }

        stage('Install & Lint & Build'){
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
        environment {
            PATH = "/usr/bin:/usr/local/bin:/bin:/usr/sbin:/sbin"
        }
        steps {
            sh 'docker version'   // test
            sh 'node -v && npm -v'
            sh 'npm install'
            sh 'npm run build'
        }
}
    }

    post {
        success {
            archiveArtifacts artifacts: 'dist/**/*', fingerprint: true, allowEmptyArchive: false
        }
        failure {
            echo 'Pipeline thất bại - xem log từng stage (Install & Lint & Build).'
        }
    }
}