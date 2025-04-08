pipeline {
    agent any

    environment {
        // Diretórios principais
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
        PYTHON = 'python3'
    }

    stages {

        stage('Clonar Repositório') {
            steps {
                echo 'Clonando repositório...'
                // O checkout padrão já ocorre automaticamente, então essa etapa é simbólica
            }
        }

        stage('Instalar dependências do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo 'Limpando e instalando dependências do frontend...'
                    // Remove módulos antigos e limpa cache do npm
                    sh '''
                    rm -rf node_modules package-lock.json
                    npm cache clean --force
                    npm install
                    '''
                }
            }
        }

        stage('Build do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo 'Executando build do frontend...'
                    sh 'npm run build'
                }
            }
        }

        stage('Instalar dependências do backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo 'Criando ambiente virtual e instalando dependências do backend...'
                    sh '''
                    rm -rf venv
                    ${PYTHON} -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Rodar o backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo 'Iniciando backend Flask...'
                    sh '''
                    . venv/bin/activate
                    export FLASK_APP=app.py
                    flask run --host=0.0.0.0 --port=5000 &
                    '''
                }
            }
        }
    }
}
