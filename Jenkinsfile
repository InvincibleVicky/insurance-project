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

        stage("Docker Installation") {
            steps {
                echo "Running Ansible Playbook for Docker Installation"
                ansiblePlaybook credentialsId: "ansible-ssh",
                                disableHostKeyChecking: true,
                                installation: 'ansible',
                                inventory: '/etc/ansible/hosts',
                                playbook: 'ansible-playbook.yml',
                                vaultTmpPath: ''
            }

        stage("Docker Image Creation") {
            steps { 
                echo "Building Docker Image"
                sh "docker build -t insurance-img:v1 ."
            }
        }

        stage('port expose'){
            steps{
                sh 'docker run -dt -p 8082:8082 --name Container1 insurance-img:v1'
            }
        }   
    }
}
