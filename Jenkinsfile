pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Amanda-Brit0/teste-jenkins.git'
        BRANCH = 'teste-jenkins-amanda-brito'
        GIT_CREDENTIALS = 'github-pat-amanda'
    }

    triggers {
        // Executa automaticamente a cada hora
        cron('H * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${BRANCH}",
                    url: "${GIT_REPO}",
                    credentialsId: "${GIT_CREDENTIALS}"
            }
        }

        stage('Verificar alterações') {
            steps {
                script {
                    sh """
                        echo "====================================="
                        echo "Verificando alterações no repositório..."
                        git pull origin ${BRANCH}

                        CHANGES=\$(git status --porcelain | wc -l)

                        if [ \$CHANGES -gt 0 ]; then
                            echo "====================================="
                            echo "Alterações detectadas! Arquivos modificados:"
                            git status --porcelain
                            echo "====================================="
                            git config user.email "amanda.s.brito@hotmail.com"
                            git config user.name "Amanda Brito"
                            git add .
                            git commit -m "Atualização automática via Jenkins"
                            git push origin ${BRANCH}
                            echo "Push realizado com sucesso!"
                        else
                            echo "Nenhuma alteração detectada. Nada a fazer."
                        fi
                    """
                }
            }
        }
    }
}
