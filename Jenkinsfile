pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mariamash1/ci_cd_py.git']])
            }
        }
        stage('Create virtual environment') {
            steps {
                // Create a virtual environment
                sh 'python3 -m venv venv'
            }
        }
         stage('Install Dependencies') {
            steps {
                sh '''
                    . venv/bin/activate 
                    pip install -r requirements.txt
                '''
            }
        }       
        stage('Run Tests') {
            steps {
                // Direct use of pytest from within the virtual environment
                sh './venv/bin/python3 tests11.py'
            }
        }
        stage('Update Remote Repository') {
    steps {
        script {
            // Define the SSH URL for the GitHub repository
            GITHUB_SSH_URL = "git@github.com:mariamash1/ci_cd_pytest.git"
            GITHUB_BRANCH = "main"
            GITHUB_MERGE_BRANCH = "dev"

            // Perform Git operations
            sh(script: """
                export GIT_SSH_COMMAND='ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no'

                # Switch to the target branch where you want to merge changes
                git checkout ${GITHUB_BRANCH}

                # Merge the 'dev' branch into the current branch
                git merge ${GITHUB_MERGE_BRANCH}

                # Push changes back to the remote repository using the SSH URL
                git push ${GITHUB_SSH_URL} ${GITHUB_BRANCH}
            """, returnStdout: true)
        }
    }
}
