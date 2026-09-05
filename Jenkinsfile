pipeline {
    agent {
        label '1C'
    } 

    environment {
        envString = 'true'
        STORAGE_PATH = 'C:\\Gitstorage\\Storage'
        GIT_REPO_PATH = 'C:\\Gitstorage\\Gitst'
        USER_1C = 'admin'
    }

    stages {
        stage('Gitsync') {
            steps {
                bat 'chcp 65001\n gitsync sync --storage-user "%USER_1C%" "%STORAGE_PATH%" "%GIT_REPO_PATH%"' 
            }
        }
        stage('Build test base') {
            steps {
                bat 'chcp 65001\n vrunner init-dev' 
            }
        }

        stage('Syntax check') {
            steps {
                bat 'chcp 65001\n vrunner syntax-check'                
             }       
        }
    }
    

    post {
        success {
            bat 'echo success'
        }
        failure {
            bat 'echo failure'
        }
        always {
            bat 'echo failure'
        }
    }
}