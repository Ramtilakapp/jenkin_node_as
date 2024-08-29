pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository into the workspace
                git url: 'https://github.com/Ramtilakapp/jenkin_node_as.git', branch: 'master'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
                    // Print the current user
                    sh 'whoami'

                    // Define directories
                    def localDir = '/var/lib/jenkins/workspace/node_jenkins_appli_as'
                    def deployDir = '/home/jenkins/node_jenkins_appli_as'

                    // Remove existing directory if it exists
                    sh "rm -rf ${deployDir}"

                    // Copy files from workspace to /home/jenkins/node-pipeline
                    sh "cp -r ${localDir} /home/jenkins/"

                    // Deploy to remote server using rsync
                    // sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/id_ed25519" ${deployDir} abcd@10.0.2.130:/home/ssm-user/'
                    sh 'scp -i /home/jenkins/.ssh/id_ed25519 -r /home/jenkins/node_jenkins_appli_as abcd@10.0.2.130://home/abcd/'

                    // Change ownership of remote directory
                    // sh 'ssh -i "/home/jenkins/id_ed25519" ssm-user@10.0.4.172 "sudo chown -R ssm-user:ssm-user /home/ssm-user/node-pipeline"'
                    sh 'ssh -i "/home/jenkins/.ssh/id_ed25519" abcd@10.0.2.130 "sudo mkdir -p /home/abcd/node_jenkins_appli_as && sudo chmod -R 777 /home/abcd/node_jenkins_appli_as/.git/ && sudo chown -R abcd:abcd /home/abcd/node_jenkins_appli_as"'


                    // Restart PM2 process
                    sh 'ssh -i "/home/jenkins/id_ed25519" abcd@10.0.2.130 "pm2 stop /home/abcd/node_jenkins_appli_as/app.js && pm2 start /home/abcd/node_jenkins_appli_as/app.js"'
                }
            }
        }
    }
}
