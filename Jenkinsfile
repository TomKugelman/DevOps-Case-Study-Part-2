pipeline {
    agent none 
    stages {
        stage("Clone repository into workspace") {
            agent none
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/TomKugelman/DevOps-Case-Study-Part-2']]])
            }
        }
        stage('Testing Goes here') {
            agent none
            steps {
                echo 'No tests to run'
            }
        }
        stage('Install and Apply') {
            agent none
            steps {
                // This playbook will install metricbeat and apply the kubernetes.yaml to the already created cluster on target machine
                ansiblePlaybook installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: './ansible/main.yml'
            }
        }
    }
}