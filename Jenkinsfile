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
                sh 'ssh-add ~/crossovertest.pem'
            }
        }
        stage('Build Image') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/build.yml'
            }
        }
        stage('Redis Power ON') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/replicate1.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/replicate2.yml'
            }
        }
        stage('Build VotingAPP') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/app1.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/app2.yml'
            }
        }
        stage('Load Nginx Balancer') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/webserver1.yml'
            }
        }
        stage('Load Grafana Monitoring') {
            steps {
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/exporter.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/rexporter.yml'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansibletest/inventory.txt', playbook: '/var/lib/jenkins/workspace/ansibletest/grafana.yml'
            }
        }
    }
}
