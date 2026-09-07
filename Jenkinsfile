pipeline {
    agent {
        label '1C'
    } 

    environment {
        envString = 'true'
        STORAGE_PATH = 'C:\\Gitstorage\\Storage'
        GIT_REPO_PATH = 'C:\\Gitstorage\\Gitst'
        USER_1C = 'admin'
        EPF_STORAGE_PATH = 'C:\\OtusRepo\\OtusProject\\build\\epf'
        GIT_EPF_STORAGE_PATH = 'C:\\OtusRepo\\OtusProject\\src\\epf'
    }

    stages {
        stage('Synchronize repo') {
            steps {
                bat 'chcp 65001\n gitsync sync --storage-user "%USER_1C%" "%STORAGE_PATH%" "%GIT_REPO_PATH%"' 
            }
        }

        stage('Synchronize data processors') {
            steps {
                bat 'chcp 65001\n precommit1c --decompile "%EPF_STORAGE_PATH%" "%GIT_EPF_STORAGE_PATH%"' 
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

        stage('Vanessa') {
            steps{
                script {
                    try {
                        bat "chcp 65001\n runner vanessa"
                    }
                    catch(Exception Exc) {
                        currentBuild.result = 'UNSTABLE'
                    }
                }   
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
            allure includeProperties: false, jdk: '', resultPolicy: 'LEAVE_AS_IS', results: [[path: 'out/syntax-check/allure'], [path: 'out/smoke/allure']]
            junit 'out/syntax-check/junit/junit.xml'   
            junit 'out/smoke/junit/*.xml'
        }
    }

    options {
        skipDefaultCheckout(true)
        }
}