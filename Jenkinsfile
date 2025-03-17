pipeline {
    agent any
    
    stages {
        stage("Git Checkout") {
            steps {
                echo "Cloning Git Repo"
                git url: "https://github.com/InvincibleVicky/insurance-project/"
            }
        }

        stage("Compile the Code") {
            steps {
                echo "Starting Compilation"
                sh "mvn compile"
            }
        }

        

        stage("Create a Package") {
            steps {
                echo "Packaging Application"
                sh "mvn package"
            }
        }

        stage("Docker Image Creation") {
            steps { 
                echo "Building Docker Image"
                sh "docker build -t insurance-img:v1 ."
            }
        }

        stage('Ansbile config and Deployment') {
           steps {
                ansiblePlaybook credentialsId: 'ansible-ssh', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', extras: '--private-key /var/lib/jenkins/workspace/insure-me/my-key', vaultTmpPath: ''
 
            }
        }

    }
}
