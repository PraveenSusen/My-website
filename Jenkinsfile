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
                sshagent(credentials: ['ec2-key']) {

                    bat '''
                    scp -o StrictHostKeyChecking=no index.html ubuntu@%EC2_HOST%:/tmp/

                    ssh -o StrictHostKeyChecking=no ubuntu@%EC2_HOST% "sudo cp /tmp/index.html /var/www/html/index.html"
                    '''
                }
            }
        }
    }
}