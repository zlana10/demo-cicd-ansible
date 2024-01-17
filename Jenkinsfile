pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
    stages {

        stage('Run Ansible') {
            steps {
                withCredentials([file(credentialsId: 'aws-ec2-key', variable: 'aws-ec2-key')]) {
                    sh 'ls -la'
                    sh 'cp /${aws-ec2-key} aws-ec2-key'
                    sh 'cat ansible_key'
                    sh 'ansible --version'
                    sh 'ls -la'
                    sh 'chmod 400 ansible_key '
                    sh 'ansible-playbook -i hosts --private-key aws-ec2-key playbook.yml'
                }
            }
        }

    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}