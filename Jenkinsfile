pipeline {
    agent any

    environment {
        EC2_HOST = "13.54.153.39"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy Website') {
            steps {

                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'ec2-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {

                    bat '''
                    scp -i "%SSH_KEY%" -o StrictHostKeyChecking=no *.html %SSH_USER%@%EC2_HOST%:/tmp/
                    scp -i "%SSH_KEY%" -o StrictHostKeyChecking=no *.css %SSH_USER%@%EC2_HOST%:/tmp/

                    ssh -i "%SSH_KEY%" -o StrictHostKeyChecking=no %SSH_USER%@%EC2_HOST% "sudo cp /tmp/*.html /var/www/html/ && sudo cp /tmp/*.css /var/www/html/"
                    '''
                }
            }
        }
    }
}