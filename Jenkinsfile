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

   //     stage('Install Ansible Collections') {
   //         steps {
   //             sh 'ansible-galaxy collection install -r requirements.yml --force'
   //         }
   //     }
        
        stage('Deploy Infrastructure') {
            steps {
                sh 'ansible-playbook playbooks/site.yml -vv'
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
