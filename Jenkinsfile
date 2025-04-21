pipeline {
    agent { label 'Build' }
    stages {
        stage ("Install Tools") {
            steps {
                sh "sudo apt-get update && sudo apt-get install docker.io git -y"
                sh "sudo systemctl start docker"  // Start Docker service using systemctl
            }
        }
        stage ("Clone and Build Image") {
            steps {
                sh "git clone https://github.com/deepak-devautomation09/Jenkins_project.git"
                dir('Jenkins_project') { // Ensure you're in the correct directory for Docker build
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
        stage("Clean Up") {
            steps {
                cleanWs() // Cleans up the workspace at the end
            }
        }
    }
}
