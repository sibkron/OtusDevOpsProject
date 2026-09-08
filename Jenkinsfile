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
        BUILD_NUMBER = '1'
    }

    stages {
        stage('Синхронизация репозитория') {
            steps {
                bat 'chcp 65001\n gitsync sync --storage-user "%USER_1C%" "%STORAGE_PATH_CFE%" "%GIT_REPO_PATH_CFE%"' 
            }
        }

        stage('Сборка тестовой базы') {
            steps {
                bat 'chcp 65001\n vrunner init-dev' 
            }
        }

        stage('Сборка расширения') {
            steps {
                bat 'chcp 65001\n vrunner compileext ./src/cfe --extensionName Yaxunit' 
            }
        }

        stage('Синтаксический контроль') {
            steps {
                bat 'chcp 65001\n vrunner syntax-check'                
             }       
        }

        stage('Yaxunit тесты') {
            steps {
                bat 'chcp 65001\n "C:\\Program Files\\1cv8\\8.3.27.1508\\bin\\1cv8c.exe" ENTERPRISE /IBConnectionString "File=""C:\\Base1c\\DemoUnitTest"";" /C "RunUnitTests=C:\\OtusRepo\\OtusProject\\tools\\Yaxunit.json" /N"Администратор"\n'                
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
            allure includeProperties: false, jdk: '', resultPolicy: 'LEAVE_AS_IS', results: [[path: 'out/syntax-check/allure'], [path: 'out/yaxunit/allure']]
        }
    }

    options {
        skipDefaultCheckout(true)
        }
}