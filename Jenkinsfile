pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('f4da6c65-a105-43c3-9946-52a4796851a7') // ID da credencial
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/xnetuno/hello-world.git'
            }
        }
        stage('Validate') {
            steps {
                script {
                    // Validação executando o script
                    sh 'python3 main.py'
                }
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    // Construir a imagem Docker
                    sh 'docker build -t hello-world-image .'
                }
            }
        }
        stage('Run Docker container') {
            steps {
                script {
                    // Rodar o container
                    sh 'docker run -d --name hello-world-container hello-world-image'
                }
            }
        }
        stage('Tag Docker image') {
            steps {
                script {
                    // Taguear a imagem
                    sh 'docker tag hello-world-image $DOCKERHUB_CREDENTIALS_USR/hello-world-image:v1'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Fazer login no Docker Hub e fazer o push da imagem
                    sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                    docker push $DOCKERHUB_CREDENTIALS_USR/hello-world-image:v1
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                // Remover o container depois que terminar
                sh 'docker rm -f hello-world-container || true'
            }
        }
    }
}
