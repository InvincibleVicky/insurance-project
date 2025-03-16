pipeline{
    agent any
        tools{
            maven "M2_HOME"
        }

    stages{

        stage("Git Checkout"){
            steps{
                echo "Cloning Git Repo"
                git url: "https://github.com/InvincibleVicky/insurance-project/"
            }
        }

        stage("compile the code"){
            steps{
                echo "starting compiling"
                sh "mvn compile"
            }

        stage("Testing the Code"){
            steps{
                echo "Testing the Code"
                sh "mvn test"
            }
        }

        stage("QA"){
            steps{
                sh "mvn checkstyle:checkstyle"
            }
        }

        stage("Create a Package"){
            steps{
                echo "Packaging"
                sh "mvn package"
            }
        }

        stage("Docker Image Creation"){
            step{
                sh "docker build -t insurance-img ."
            }
        }

        stage("Port Expose"){
            step{
                sh "docker run -dt -p 8081:8081 --name container1 insurance-img"
            }
        }

        stage("Ansible Config and Deployment"){
            step{
                ansiblePlaybook credentialsId: "ansible-ssh", disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
