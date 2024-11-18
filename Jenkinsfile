pipeline {
    agent any
    tools {
        maven 'Maven' 
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/kenzahatem/java-hello-world-with-maven',
                credentialsId: 'github-pat'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement effectué avec succès'
            }
        }
        stage('Tag and Push') {
            steps {
                script {
                    // Générer un tag avec la date et l'heure
                    def tagName = "v1.2." + new Date().format("yyyyMMdd-HHmm")
                    echo "Creating and pushing tag: ${tagName}"

                    // Ajouter les commandes Git pour créer et pousser le tag
                    withCredentials([usernamePassword(credentialsId: 'github-pat', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh """
                            git config --global user.name "Jenkins"
                            git config --global user.email "jenkins@example.com"
                            git tag -a ${tagName} -m "Automated tag by Jenkins"
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/kenzahatem/java-hello-world-with-maven ${tagName}
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline terminé avec succès.'
        }
        failure {
            echo 'Pipeline échoué.'
        }
    }
}
