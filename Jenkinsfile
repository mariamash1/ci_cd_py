pipeline {
    agent any

    stages {
        stage('Clone GitHub Repo') {
            steps {
                checkout scm
            }
        }
        stage('Setup Python Virtual Environment') {
            steps {
                script {
                    // Check if the venv directory exists
                    if (fileExists('venv')) {
                        echo 'Virtual environment already exists'
                    } else {
                        sh 'python3 -m venv venv --system-site-packages'                      
                        sh 'source venv/bin/activate'                        
                        sh 'pip install --upgrade pip'
                    }
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest test11.py
                '''
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
