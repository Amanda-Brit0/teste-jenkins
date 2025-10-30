pipeline {
    agent any

    triggers {

        cron('0 */2 * * *')
    }

    stages {
        stage('Sincronizar e verificar mudanças') {
            steps {
                script {
                    echo "🔄 Iniciando varredura do repositório..."
                    sh 'git config --global user.email "jenkins@example.com"'
                    sh 'git config --global user.name "Jenkins Bot"'

                    def changes = sh(script: "git pull origin teste-jenkins-amanda-brito || true", returnStdout: true).trim()

                    if (changes.contains('Already up to date')) {
                        echo "✅ Nenhuma alteração detectada. Nada a fazer."
                    } else {
                        echo "📝 Alterações detectadas! Realizando commit e push..."
                        sh '''
                            git add .
                            git commit -m "Atualização automática do Jenkins ($(date +'%d/%m %H:%M'))" || echo "Sem mudanças para commitar."
                            git push origin teste-jenkins-amanda-brito
                        '''
                    }
                }
            }
        }

        stage('Finalização') {
            steps {
                echo "🚀 Pipeline executada com sucesso!"
            }
        }
    }
}
