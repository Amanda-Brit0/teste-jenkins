pipeline {
    agent any

    triggers {
        cron('0 23,12 * * *') 
    }

    stages {
        stage('Checkout') {
            steps {
                // Configuração inicial e obtenção do código
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/teste-jenkins-amanda-brito']], 
                    userRemoteConfigs: [[
                        url: 'https://github.com/Amanda-Brit0/teste-jenkins.git'
                    ]]
                ])
                
                // Configurações do Git para o commit
                sh 'git config user.email "amanda-brito-jenkins@example.com"'
                sh 'git config user.name "Jenkins Bot Amanda Brito"'
            }
        }
        
        stage('Gerar Alteracao Local') {
            steps {

                sh 'echo "Execução forçada em: $(date)" >> pipeline_log.txt'
            }
        }

        stage('Commit e Push Condicional') {
            steps {
                script {
                    sh 'git add .'
                    

                    def changes_to_commit = sh(script: 'git diff --staged --quiet || echo "changes"', returnStdout: true).trim()

                    if (changes_to_commit.contains('changes')) {
                        

                        withCredentials([usernamePassword(
                            credentialsId: 'github-pat-amanda', 
                            passwordVariable: 'GIT_TOKEN', 
                            usernameVariable: 'GIT_USERNAME'
                        )]) {
                            sh "git commit -m \"Atualização automática do Jenkins ($(date +'%d/%m %H:%M'))\""
                            
                            sh "git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/Amanda-Brit0/teste-jenkins.git teste-jenkins-amanda-brito"
                        }
                    }
                }
            }
        }
    }
}