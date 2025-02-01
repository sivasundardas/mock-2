pipeline{
    agent any

    stages{
        stage('docker install'){
            steps{ 
                script {
                    def dockerInstalled = sh(script: 'which docker', returnStatus: true) == 0
                    if (!dockerInstalled) {
                        sh '''
                        sudo yum update -y
                        sudo yum install -y docker
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        sudo usermod -aG docker jenkins
                        '''
                    }
                }
            }
        }
        stage('pull'){
            steps{ 
                sh 'docker pull nginx'
            }
        }
        stage('run'){
            steps{
                sh 'docker run -it -d --name das -p 8000:80 nginx'
            }
        }
        stage('backup'){
            steps{
                sh 'docker commit das backup'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag backup sivasundardas/mock3'
            }
        }
        stage('push'){
            steps{
                 sh 'echo "Dascme@123" | docker login -u "sivasundardas" --password-stdin'
                sh 'docker push sivasundardas/mock3'
            }
        }
    }
    post{
        always{
            sh 'docker rmi -f nginx'
        }
        success{
            sh 'docker rm -f das'
        }
    }
    
}