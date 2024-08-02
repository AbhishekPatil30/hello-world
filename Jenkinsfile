pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    environment {
        TOMCAT_USER = 'ec2-user'
        TOMCAT_HOST = '3.109.210.239'
        TOMCAT_PATH = '/opt/tomcat/apache-tomcat-9.0.68/webapps/'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'gittoken', url: 'https://github.com/AbhishekPatil30/hello-world.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['tomcat_ssh']) {
                     sh "rsync -avz -e 'ssh -o StrictHostKeyChecking=no' --delete target/*.war ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_PATH}"
                }
            }
        }
    }
}
