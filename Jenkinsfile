pipeline {
    agent {
        label '1C'
    } 

    environment {
        envString = 'true'
    }

    stages {
        stage('Synchronize') {
            steps {
                bat 'chcp 65001\n gitsync sync --storage-user admin C:/Gitstorage/Storage C:/Gitstorage/Gitst' 
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