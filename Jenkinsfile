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

        stage("deploy"){
            steps{
                sh "docker run -dt -p 8081:8081 --name c01 insurance-img:v1"
            }
        }

    }
}
