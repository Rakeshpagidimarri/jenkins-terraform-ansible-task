pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws') // ID from Jenkins credentials store
        AWS_SECRET_ACCESS_KEY = credentials('aws') // ID from Jenkins credentials store
     }

    stages {
        

        stage('Checkout') {
            steps {
                deleteDir()
                sh 'echo cloning repo'
                sh 'git clone https://github.com/Rakeshpagidimarri/jenkins-terraform-ansible-task.git' 
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    dir('/var/lib/jenkins/workspace/ansible-ra/jenkins-terraform-ansible-task') {
                    sh 'pwd'
                    sh 'terraform init'
                    // sh 'terraform destroy -auto-approve'
                    sh 'terraform plan'
                    sh 'terraform apply -auto-approve'
                    }
                }
            }
        }
        
        stage('Ansible Deployment') {
            steps {
                script {
                   sleep '360'
                    ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-ra/jenkins-terraform-ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-ra/jenkins-terraform-ansible-task/amazon-playbook.yml', vaultTmpPath: ''
                    ansiblePlaybook becomeUser: 'ubuntu', credentialsId: 'ubuntuuser', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-ra/jenkins-terraform-ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-ra/jenkins-terraform-ansible-task/ubuntu-playbook.yml', vaultTmpPath: ''
                }
            }
        }
    }
}
