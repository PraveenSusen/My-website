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
                sshagent(['ec2-key']) {

                    sh """
                    scp -o StrictHostKeyChecking=no *.html ubuntu@$EC2_HOST:/tmp/
                    scp -o StrictHostKeyChecking=no *.css ubuntu@$EC2_HOST:/tmp/
                    """

                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@$EC2_HOST '
                        sudo cp /tmp/*.html /var/www/html/
                        sudo cp /tmp/*.css /var/www/html/
                    '
                    """
                }
            }
        }
    }
}