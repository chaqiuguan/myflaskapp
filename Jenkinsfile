pipeline {
    agent any

    environment {
        // ① 换成你的 Python 路径
        PYTHON = 'D:\\python\\python3.12\\python.exe'
    }

    stages {
        stage('Verify Python') {
            steps {
                bat """
                    chcp 65001 >nul
                    ${PYTHON} --version
                    echo Python OK
                """
            }
        }
        stage('Checkout') {
            steps {
                // ② 换成你自己的仓库地址
                git url: 'https://github.com/chaqiuguan/myflaskapp.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat """
                    ${PYTHON} -m pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple
                    ${PYTHON} -m pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
                    ${PYTHON} -m pip install pytest pytest-cov flake8 pyinstaller -i https://pypi.tuna.tsinghua.edu.cn/simple
                """
            }
        }
        stage('Lint') {
            steps {
                bat "${PYTHON} -m flake8 app.py tests/"
            }
        }
        stage('Test') {
            steps {
                bat "${PYTHON} -m pytest --cov=app tests/ --cov-report=html"
            }
            post {
                always {
                    publishHTML(target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'htmlcov',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        stage('Build') {
            steps {
                bat "${PYTHON} -m PyInstaller --onefile app.py"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy stage placeholder (Windows 服务下 start 启动的进程会随构建结束退出)'
            }
        }
    }
    post {
        success { echo 'CI/CD pipeline completed successfully!' }
        failure { echo 'CI/CD pipeline failed!' }
    }
}