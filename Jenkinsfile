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
                    sh 'docker rmi hungltse04132/demo-cicd-ansible'
                    sh 'docker build -t hungltse04132/demo-cicd-ansible .'
                    sh 'docker push hungltse04132/demo-cicd-ansible'
                    sh 'sleep 20'
                    sh 'docker image pull hungltse04132/demo-cicd-ansible'
                    sh 'docker container stop demo-cicd-ansible || echo "this container does not exist" '
                    sh 'docker run --name demo-cicd-ansible hungltse04132/demo-cicd-ansible'
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