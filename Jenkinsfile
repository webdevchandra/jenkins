// Jenkinsfile

// 1. Define the sample deployment directory on the Ubuntu server
def DEPLOY_ROOT = '/var/www/html/jenkins' 

pipeline {
    // Agent: Runs the job on any available Jenkins agent/node.
    agent any 

    // Environment: Define variables and paths.
    environment {
        // Define a variable for the Git URL (used for logging/checks, not for cloning itself)
        GIT_URL = 'https://github.com/webdevchandra/jenkins.git' 
        // WORKSPACE variable is automatically provided by Jenkins.
    }
    
    stages {
        
        // --- Stage 1: Setup & Clone Verification ---
        stage('Setup and Verify Clone') {
            steps {
                echo 'Starting setup: Code cloned into Jenkins WORKSPACE.'
                // Verify the contents of the cloned directory
                sh 'echo "Listing files in Jenkins Workspace:"'
                sh 'ls -al ${WORKSPACE}' 
                // ➡️ Add dependency installation here (e.g., sh 'npm install' or 'pip install')
            }
        }

        // --- Stage 2: Code Synchronization to Server Path ---
        stage('Code Synchronization') {
            steps {
                echo "Initiating code sync to: ${DEPLOY_ROOT}"
                
                // CRITICAL: Use 'dir' to change the working directory to the target path.
                dir("${DEPLOY_ROOT}") {
                    
                    // Synchronization Step: Copies all files from the Jenkins WORKSPACE 
                    // into the DEPLOY_ROOT. This requires the Jenkins user to have 
                    // password-less write permission via sudoers or file ownership.
                    sh "rsync -av --exclude '.git' ${WORKSPACE}/ ."
                    echo 'Code synchronized successfully to server path.'

                    // ➡️ Add post-sync commands here (e.g., sh 'chmod -R 755 .' or 'npm start')
                    echo 'Deployment complete placeholder.'
                }
            }
        }
    }
    
    // --- Post-Build Actions ---
    post {
        always {
            echo 'Pipeline finished. Cleaning up workspace.'
            // Clean up the temporary directory where the Git repo was cloned
            cleanWs() 
        }
        success {
            echo '✅ Deployment succeeded.'
        }
        failure {
            echo '❌ Pipeline failed! Review the logs.'
        }
    }
}
