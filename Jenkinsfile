pipeline {
    agent { label 'Build' }
    stages {
        stage ("Install Tools") {
            steps {
                sh "sudo yum install docker git -y"
                sh "sudo service docker start"
            }
        }
        stage ("Clone and Build Image") {
            steps {
                sh "git clone https://github.com/soumyabiswas37/jenkins_project.git"
                sh "docker build -t mynewimage:v$BUILD_NUMBER ."
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
                cleanWs()
            }
        }
    }
}
