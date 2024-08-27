pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository into the workspace
                git url: 'https://github.com/ashi-2212/node-pipeline.git', branch: 'master'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
                    // Print the current user
                    sh 'whoami'

                    // Define directories
                    def localDir = '/var/lib/jenkins/workspace/node-pipeline'
                    def deployDir = '/home/jenkins/node-pipeline'

                    // Remove existing directory if it exists
                    sh "rm -rf ${deployDir}"

                    // Copy files from workspace to /home/jenkins/node-pipeline
                    sh "cp -r ${localDir} ${deployDir}"

                    // Deploy to remote server using rsync
                    sh 'rsync -avz -e "ssh -i /home/jenkins/id_ed25519" ${deployDir} ubuntu@16.16.187.59:/home/ubuntu/'

                    // Change ownership of remote directory
                    sh 'ssh -i "/home/jenkins/id_ed25519" ubuntu@16.16.187.59 "sudo chown -R ubuntu:ubuntu /home/ubuntu/node-pipeline"'

                    // Restart PM2 process
                    sh 'ssh -i "/home/jenkins/id_ed25519" ubuntu@16.16.187.59 "pm2 stop /home/ubuntu/node-pipeline/app.js && pm2 start /home/ubuntu/node-pipeline/app.js"'
                }
            }
        }
    }
}
