pipeline {
    agent none 
    environment {
        registry = "trushpatel/webchess"
        docker_user = "trushpatel"
        docker_app = "webchess"
        GOCACHE = "/tmp"
    }
    stages {
        stage('Build') {
            agent {
                kubernetes {
                    inheritFrom 'agent-template'
                }
            }
            steps{
                container('docker') {
                    checkout scm
                }
            }
        }
        stage ('Deploy') {
            agent {
                node {
                    label 'deploy'
                }
            }
            steps {
                sshagent(credentials: ['cloudlab']) {
                    sh "sed -i 's/DOCKER_USER/${docker_user}/g' webchess.yml"
                    sh "sed -i 's/DOCKER_APP/${docker_app}/g' webchesss.yml"
                    sh "sed -i 's/BUILD_NUMBER/${BUILD_NUMBER}/g' webchess.yml"
                    sh 'scp -r -v -o StrictHostKeyChecking=no *.yml trush@128.105.146.151:~/'
                    sh 'ssh -o StrictHostKeyChecking=no trush@128.105.146.151 kubectl apply -f /users/trush/deployment.yml -n jenkins'
                    sh 'ssh -o StrictHostKeyChecking=no trush@128.105.146.151 kubectl apply -f /users/trush/service.yml -n jenkins'                                        
                }
            }
        }
    }
}
