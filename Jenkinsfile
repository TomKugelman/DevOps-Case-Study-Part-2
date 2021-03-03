pipeline {
    agent none 
    stages {
        stage("Clone repository into workspace") {
            steps {
                git 'https://github.com/TomKugelman/DevOps-Case-Study-Part-2'
            }
        }
        stage('Testing Goes here') {
            steps {
                echo 'No tests to run'
            }
        }
        stage('Install and Apply') {
            steps {
                // This playbook will install metricbeat and apply the kubernetes.yaml to the already created cluster on target machine
                ansiblePlaybook installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: './ansible/main.yml'
            }
        }
    }
}