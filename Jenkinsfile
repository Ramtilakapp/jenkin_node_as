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
                    sh "cp -r ${localDir} /home/jenkins/"

                    // Deploy to remote server using rsync
                    // sh 'rsync -avz -e "ssh -i /home/jenkins/id_ed25519" ${deployDir} ssm-user@10.0.4.172:/home/ssm-user/'
                    sh 'scp -i /home/jenkins/id_ed25519 -r /home/jenkins/node-pipeline ssm-user@10.0.4.172:/home/ssm-user/'

                    // Change ownership of remote directory
                    // sh 'ssh -i "/home/jenkins/id_ed25519" ssm-user@10.0.4.172 "sudo chown -R ssm-user:ssm-user /home/ssm-user/node-pipeline"'
                    sh 'ssh -i "/home/jenkins/id_ed25519" ssm-user@10.0.4.172 "sudo mkdir -p /home/ssm-user/node-pipeline && sudo chown -R ssm-user:ssm-user /home/ssm-user/node-pipeline"'


                    // Restart PM2 process
                    sh 'ssh -i "/home/jenkins/id_ed25519" ssm-user@10.0.4.172 "pm2 stop /home/ssm-user/node-pipeline/app.js && pm2 start /home/ssm-user/node-pipeline/app.js"'
                }
            }
        }
    }
}
