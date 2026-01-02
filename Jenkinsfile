pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        ANSIBLE_CONFIG = 'ansible.cfg'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Ansible') {
            steps {
                sh 'ansible --version'
                sh 'ansible-inventory -i inventory/production --list'
            }
        }

        stage('Deploy Infrastructure') {
            steps {
                sh '''
                    ansible-playbook \
                      -i inventory/production \
                      playbooks/site.yml \
                      --user jenkins \
                      --private-key ~/.ssh/id_ed25519 \
                      -vv
                '''
            }
        }
    }

    post {
        always {
            echo 'Deployment pipeline completed.'
        }
        success {
            echo '✅ Infrastructure deployed successfully!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
