pipeline {
    agent {
        label '1C'
    } 

    environment {
        envString = 'true'
        STORAGE_PATH = 'C:\\Base1c\\Storage'
        GIT_REPO_PATH = 'C:\\OtusRepo\\OtusProject\\src\\cf'
        STORAGE_PATH_CFE = 'C:\\Base1c\\StorageYaxunit'
        GIT_REPO_PATH_CFE = 'C:\\OtusRepo\\OtusProject\\src\\cfe'
        USER_1C = 'admin'
        EPF_STORAGE_PATH = 'C:\\OtusRepo\\OtusProject\\build\\epf'
        GIT_EPF_STORAGE_PATH = 'C:\\OtusRepo\\OtusProject\\src\\epf'
        GITSYNC_EXTENSION = 'Yaxunit'
    }

    stages {
        stage('Синхронизация репозитория расширения') {
            steps {
                bat 'chcp 65001\n gitsync sync --storage-user "%USER_1C%" -e "%GITSYNC_EXTENSION%" "%STORAGE_PATH_CFE%" "%GIT_REPO_PATH_CFE%"' 
            }
        }

        stage('Сборка тестовой базы') {
            steps {
                bat 'chcp 65001\n vrunner init-dev' 
            }
        }

        stage('Синтаксический контроль') {
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
            allure includeProperties: false, jdk: '', resultPolicy: 'LEAVE_AS_IS', results: [[path: 'out/syntax-check/allure'], [path: 'out/smoke/allure']]
        }
    }

    options {
        skipDefaultCheckout(true)
        }
}