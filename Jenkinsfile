pipeline {
    agent{
        label "generic"
    } //agent
    stages {
        steps {
            sh """
                sudo pip3 install molecule
                sudo pip3 install docker
            """
        }//steps
    }//stage

    stages {
        stage ("Create docker image for testing") {
            steps {
                sh """
                    python3 -m molecule create
                """
            }//steps
            
        }//stage
        stage("Apply Ansible role to docker image") {
            steps {
                sh """
                    python3 -m molecule converge
                """
            }//steps
        }//stage   
        stage("Check idempotency") {
            steps {
                sh """
                    python3 -m molecule idempotence
                """
            }//steps
        }//stage
        stage("Cleanup moecule") {
            steps {
                sh """
                    python3 -m molecule cleanup
                """
            }//steps
        }//stage
        stage("Destroy molecule instance") {
            steps {
                sh """
                    python3 -m molecule destroy
                """
            }//steps
        }//stage
    }//stages
    post {
        always {
            sh """
                sudo pip3 uninstall docker -y
                sudo pip3 uninstall molecule -y
            """
        }//always
    }//post
} //pipeline
