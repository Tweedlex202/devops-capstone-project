pipeline {
    agent any
    
    environment {
        FLASK_APP = 'wsgi:app'
        DATABASE_URI = 'postgresql://postgres:postgres@localhost:5432/testdb'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📥 Получение кода из GitHub...'
                checkout scm
            }
        }
        
        stage('Setup Python Environment') {
            steps {
                echo '🐍 Настройка Python окружения...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }
        
        stage('Lint Code') {
            steps {
                echo '🔍 Проверка качества кода...'
                sh '''
                    . venv/bin/activate
                    pip install flake8 pylint || true
                    flake8 service tests --count --max-line-length=127 --statistics || true
                '''
            }
        }
        
        stage('Run Unit Tests') {
            steps {
                echo '🧪 Запуск unit тестов (test_models.py)...'
                sh '''
                    . venv/bin/activate
                    export FLASK_APP=wsgi:app
                    pytest tests/test_models.py -v --tb=short --color=yes
                '''
            }
        }
        
        stage('Run Integration Tests') {
            steps {
                echo '🧪 Запуск интеграционных тестов (test_routes.py)...'
                sh '''
                    . venv/bin/activate
                    export FLASK_APP=wsgi:app
                    pytest tests/test_routes.py -v --tb=short --color=yes
                '''
            }
        }
        
        stage('Code Coverage Report') {
            steps {
                echo '📊 Генерация отчёта о покрытии кода...'
                sh '''
                    . venv/bin/activate
                    pytest tests/ --cov=service --cov-report=term --cov-report=html --cov-report=xml
                    echo "Покрытие кода сгенерировано в папке htmlcov/"
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo '🐳 Сборка Docker образа...'
                script {
                    sh '''
                        docker build -t devops-capstone:${BUILD_NUMBER} .
                        docker tag devops-capstone:${BUILD_NUMBER} devops-capstone:latest
                        echo "Docker образ собран: devops-capstone:${BUILD_NUMBER}"
                    '''
                }
            }
        }
        
        stage('Security Check') {
            steps {
                echo '🔒 Проверка безопасности зависимостей...'
                sh '''
                    . venv/bin/activate
                    pip install safety || true
                    safety check || echo "Предупреждение: найдены уязвимости"
                '''
            }
        }
    }
    
    post {
        always {
            echo '🧹 Очистка рабочего пространства...'
            sh '''
                rm -rf venv
                rm -rf .pytest_cache
                rm -rf __pycache__
            '''
        }
        success {
            echo '✅ =========================================='
            echo '✅ Pipeline успешно завершен!'
            echo '✅ Все тесты прошли успешно'
            echo '✅ =========================================='
        }
        failure {
            echo '❌ =========================================='
            echo '❌ Pipeline завершился с ошибкой!'
            echo '❌ Проверьте логи выше'
            echo '❌ =========================================='
        }
    }
}