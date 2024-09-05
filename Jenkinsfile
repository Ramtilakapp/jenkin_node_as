pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository into the workspace
                git url: 'https://github.com/Ramtilakapp/jenkin_node_as.git', branch: 'master'
            }
        }
        stage('Deployment') {
            steps {
                 sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/id_ed25519" /var/lib/jenkins/workspace/test_deploy_node abcd@10.0.2.130:/var/www/html'
            }
        }

        
    }
}
