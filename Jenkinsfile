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
                    sh 'echo "hungbeo003 | docker login -u hungltse04132@gmail.com --password-stdin" docker.io'
                    sh 'docker image pull hungltse04132/demo-cicd-ansible'
                    sh 'docker network create dev || echo "this network exists"'
                    sh 'docker container stop demo-cicd-ansible || echo "this container does not exist" '
                    sh 'echo y | docker container prune '
                    sh 'docker run -it --name demo-cicd-ansible --network dev hungltse04132/demo-cicd-ansible'
                }
            }
        }
        stage('Run Ansible') {
            steps {
                withCredentials([file(credentialsId: 'ansible_key', variable: 'ansible_key')]) {
                    sh 'ls -la'
                    sh 'cp /${ansible_key} ansible_key'
                    sh 'cat ansible_key'
                    sh 'ansible --version'
                    sh 'ls -la'
                    sh 'chmod 400 ansible_key '
                    sh 'ansible-playbook -i hosts --private-key ansible_key playbook.yml'
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