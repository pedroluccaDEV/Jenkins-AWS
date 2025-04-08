# Automatização de CI/CD com Jenkins hospedado na AWS

## 🌐 Visão Geral
Essa documentação descreve o processo completo de configuração de um pipeline de CI/CD utilizando o **Jenkins**, hospedado em uma instância **EC2** da **AWS**, para realizar automação de build e deploy de um repositório hospedado no **GitHub**.

---

## ✅ Requisitos
- Conta na AWS
- Chave PEM de acesso a uma instância EC2 (Ubuntu)
- Repositório GitHub com aplicação frontend/backend
- Jenkinsfile configurado no repositório

---

## 🌍 1. Criar instância EC2
1. Acesse o **AWS Console** e crie uma instância EC2 com o **Ubuntu 22.04**.
2. Configure o **Security Group** liberando as portas:
   - **22** (SSH)
   - **8080** (Jenkins)
   - **80** e **443** (se for usar servidor web futuramente)

> **[PRINT 1: Tela de criação da instância EC2 com configuração de rede]**

---

## ⚖️ 2. Conectar via SSH na instância
No terminal:
```bash
ssh -i "/caminho/para/sua/jenkins-key.pem" ubuntu@<IP_PUBLICO>
```

---

## 🚀 3. Instalar o Jenkins na instância
Execute os seguintes comandos:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre -y

wget -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \/usr\/share\/keyrings\/jenkins-keyring.asc > \/dev\/null
echo "deb [signed-by=\/usr\/share\/keyrings\/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/" | sudo tee \/etc\/apt\/sources.list.d\/jenkins.list > \/dev\/null

sudo apt update
sudo apt install jenkins -y

sudo systemctl start jenkins
sudo systemctl enable jenkins

sudo ufw allow 8080
sudo ufw enable
```

> **[PRINT 2: Jenkins rodando na porta 8080]**

---

## 🔐 4. Acessar o Jenkins via navegador
Abra `http://<IP_PUBLICO>:8080`

Para desbloquear:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

> **[PRINT 3: Tela de setup inicial do Jenkins pedindo senha]**

Complete a instalação com os plugins sugeridos e crie um usuário admin.

---

## 📁 5. Criar um novo pipeline
1. Clique em **"Criar novo Job"** > **Pipeline**
2. Nomeie como quiser, ex: `simple-crud`
3. Configure:
   - Repositório GitHub: `https://github.com/seu-user/seu-repo`
   - Branch: `main`
   - Use o `Jenkinsfile` do repositório

> **[PRINT 4: Tela de configuração do pipeline apontando pro GitHub]**

---

## 👨‍💼 6. Jenkinsfile (exemplo)
```groovy
pipeline {
    agent any
    
    stages {
        stage('Clonar Repositório') {
            steps {
                echo 'Clonando repositório...'
            }
        }

        stage('Instalar dependências do frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build do frontend') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Instalar dependências do backend') {
            steps {
                dir('backend') {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Rodar o backend') {
            steps {
                dir('backend') {
                    sh 'python app.py'
                }
            }
        }
    }
}
```

---

## 🔄 7. Configurar Webhook no GitHub
1. Acesse as configurações do seu repositório GitHub > **Webhooks**
2. Adicione um webhook:
   - Payload URL: `http://<IP_PUBLICO>:8080/github-webhook/`
   - Content Type: `application/json`
   - Events: **Just the push event**

> **[PRINT 5: Tela de configuração do webhook no GitHub]**

---

## 🚀 8. Testando o pipeline
1. Faça um push no repositório
2. O Jenkins irá iniciar o pipeline automaticamente

> **[PRINT 6: Execução do build com sucesso no Jenkins]**

---

## 📄 Extras: Instalando Node e Python
Caso o Jenkins não encontre os comandos `npm` ou `python`, instale:

```bash
# Node.js e npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Python3 e pip
sudo apt install -y python3 python3-pip
```

---

## 🎉 Conclusão
Agora você tem um pipeline de CI/CD rodando de forma automatizada via Jenkins, integrado ao GitHub e hospedado na AWS! Isso garante entregas mais rápidas e seguras no seu projeto.

---

Se quiser evoluir isso depois com deploy automatizado em servidor web ou containerização com Docker, tamo junto ☺️
digita aí que eu ajudo!

