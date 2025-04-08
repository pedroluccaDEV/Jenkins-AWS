pipeline {
    agent any

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
        VENV_DIR = "${BACKEND_DIR}/venv"
        PYTHON = 'python3'
        NODE_OPTIONS = '--openssl-legacy-provider'
    }

    stages {

        stage('Instalar dependências do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    sh 'npm install'
                }
            }
        }

        stage('Build do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    // NODE_OPTIONS tá setado no env, então aqui já tá safe
                    sh 'npm run build'
                }
            }
        }

        stage('Instalar dependências do backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    sh """
                        ${PYTHON} -m venv venv
                        venv/bin/pip install --upgrade pip
                        venv/bin/pip install -r requirements.txt
                    """
                }
            }
        }

        stage('Rodar o backend (debug)') {
            steps {
                dir("${BACKEND_DIR}") {
                    sh """
                        nohup venv/bin/flask run --host=0.0.0.0 --port=5000 > flask.log 2>&1 &
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizada.'
        }
    }
}
