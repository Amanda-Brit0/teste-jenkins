pipeline {
    agent any

    triggers {
        cron('H 12 * * *')  
    }

    stages {
        stage('Check for changes') {
            steps {
                script {
                    def changes = sh(script: "git pull origin teste-jenkins-amanda-brito", returnStdout: true).trim()
                    if (changes.contains('Already up to date')) {
                        echo "Nenhuma alteração detectada. Nada a fazer."
                        currentBuild.result = 'SUCCESS'
                        return
                    } else {
                        echo "Alterações detectadas: realizando commit e push..."
                        sh """
                            git add .
                            git commit -m "Atualização automática do Jenkins"
                            git push origin teste-jenkins-amanda-brito
                        """
                    }
                }
            }
        }
    }
}
