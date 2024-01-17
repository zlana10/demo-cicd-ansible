pipeline {

    agent{
        docker {
            image 'ansible/ansible:latest'
        }
    }
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
    stages {

        stage('Deploy Mysql container') {

            steps {
                withCredentials([file(credentialsId: 'aws-ec2-key', variable: 'aws-ec2-key')]) {
                    sh 'ls -la'
                    sh "cp /$aws-ec2-key aws-ec2-key"
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