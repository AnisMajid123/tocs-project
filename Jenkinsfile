pipeline {
    agent any

    environment {
        SSH_SERVER = 'apache' // Replace with the name of your SSH server in Jenkins
        REMOTE_DIR = '/var/www/html/tocs_project' // Apache server's directory
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone repository
                checkout scm
            }
        }

        stage('Transfer Files') {
            steps {
                script {
                    // Publish files over SSH
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: env.SSH_SERVER,
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '**/*', // Include all files in the repo
                                        removePrefix: '',    // Remove no prefix
                                        remoteDirectory: env.REMOTE_DIR,
                                        execCommand: """
                                        sudo systemctl restart apache2
"""      // Optional: Add commands like restarting the server
                                    )
                                ],
                                usePromotionTimestamp: false,
                                verbose: true
                            )
                        ]
                    )
                }
            }
        }
    }
}
