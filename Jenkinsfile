pipeline {
    agent { label 'Build' }

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-creds')
        AWS_SECRET_ACCESS_KEY = credentials('aws-creds')
    }

    stages {
        stage ("Install Tools") {
            steps {
                sh "sudo apt-get update && sudo apt-get install docker.io git -y"
                sh "sudo systemctl start docker"
            }
        }

        stage ("Clone and Build Image") {
            steps {
                sh "git clone https://github.com/deepak-devautomation09/Jenkins_project.git"
                dir('Jenkins_project') {
                    sh "docker build -t mynewimage:v$BUILD_NUMBER ."
                }
            }
        }

        stage("Approval") {
            steps {
                input message: 'Proceed to deploy?', ok: 'Yes'
            }
        }

        stage("Run Container") {
            steps {
                sh "docker run -d --name container_$BUILD_NUMBER -p 30$BUILD_NUMBER:3000 mynewimage:v$BUILD_NUMBER"
            }
        }

        stage("Check AWS CLI") {
            steps {
                sh "aws sts get-caller-identity"
            }
        }

        stage("Clean Up") {
            steps {
                cleanWs()
            }
        }
    }
}
