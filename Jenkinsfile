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

        stage("Testing the Code") {
            steps {
                echo "Testing the Code"
                sh "mvn test"
            }
        }

        stage("QA") {
            steps {
                echo "Running Code Quality Checks"
                sh "mvn checkstyle:checkstyle"
            }
        }

        stage("Create a Package") {
            steps {
                echo "Packaging Application"
                sh "mvn package"
            }
        }

        stage("Docker Image Creation") {
            steps { // Changed "step" to "steps"
                echo "Building Docker Image"
                sh "docker build -t insurance-img:v1 ."
            }
        }

        stage("Port Expose") {
            steps { // Changed "step" to "steps"
                echo "Exposing Port 8081"
                sh "docker run -dt -p 8081:8081 --name container1 insurance-img:v1"
            }
        }

        stage("Ansible Config and Deployment") {
            steps { // Changed "step" to "steps"
                echo "Running Ansible Playbook for Deployment"
                ansiblePlaybook credentialsId: "ansible-ssh", disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
