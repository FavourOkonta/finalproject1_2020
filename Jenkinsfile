pipeline {
    agent any

    stages {
        stage('Copying from Github') {
            steps {
                git branch: 'main', url: 'https://github.com/FavourOkonta/finalproject1_2020.git'
            }
        }
        stage('Adding to Key Ring') {
            steps {
                sh 'eval `ssh-agent -s`'
                sh 'sudo ansible-galaxy collection install community.general'
            }
        }
        stage('Build Image') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/build.yml'
            }
        }
        stage('Redis Power ON') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/replicate1.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/replicate2.yml'
            }
        }
        stage('Build VotingAPP') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/app1.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/app2.yml'
            }
        }
        stage('Load Nginx Balancer') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/webserver1.yml'
            }
        }
        stage('Load Grafana Monitoring') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/exporter.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/rexporter.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/test_main/inventory', playbook: '/var/lib/jenkins/workspace/test_main/grafana.yml'
            }
        }
    }
}
