pipeline {
    agent any  // Roda em qualquer agente disponível

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pedroluccaDEV/simple-crud.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Rodando o build..."'
                sh 'make build'  // Substitui pelo comando do teu projeto
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Executando os testes..."'
                sh 'make test'  // Substitui pelo comando dos testes
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Fazendo o deploy..."'
                sh './deploy.sh'  // Se tiver um script de deploy
            }
        }
    }
}
