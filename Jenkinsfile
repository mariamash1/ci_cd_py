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
                    . venv/bin/python3 
                   -m pip install -r requirements.txt
                '''
            }
        }       
        stage('Run Tests') {
            steps {
                // Direct use of pytest from within the virtual environment
                sh './venv/bin/python3 test11.py'
            }
        }
        stage('Update Remote Repository') {
            steps {
                script {
                    // Merge the 'dev' branch into the 'main' branch
                    GITHUB_URL = "https://github.com/mariamash1/ci_cd_pytest.git"
                    GITHUB_BRANCH = "main"
                    GITHUB_MERGE_BRANCH = "dev"

                    sh(script: """
                        git config --global user.email "your-email@example.com"
                        git config --global user.name "Your Name"
                        git checkout ${GITHUB_BRANCH}
                        git merge ${GITHUB_MERGE_BRANCH}
                        git push https://${GITHUB_CREDENTIALS}@${GITHUB_URL} ${GITHUB_BRANCH}
                    """, returnStdout: true)
                }
            }
        }
    }
}
