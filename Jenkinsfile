pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
    stages {
        stage('Build and Publish Ansible Image') {
            steps {
                script {
                    // Set PATH explicitly
                    def dockerHome = tool 'docker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"

                    // Verify Docker version
                    sh 'docker --version'
                    sh 'echo "hungbeo003 | docker login -u hungltse04132@gmail.com --password-stdin"'
                    sh 'docker login -u "hungltse04132@gmail.com" -p "hungbeo003" docker.io'
                    sh 'docker run -it ansible'
                }
            }
        }
        stage('Run Ansible') {
            steps {
                withCredentials([file(credentialsId: 'aws-ec2-key', variable: 'aws-ec2-key')]) {
                    sh 'ls -la'
                    sh 'cp /$aws-ec2-key aws-ec2-key'
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